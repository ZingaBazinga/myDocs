---
tags: [junior, grpc, protobuf, backend]
---

# gRPC — Junior

← [[00-MOC-Backend-Review]] · Матрица: [[Требования к грейдам Backend Go-разработчика#gRPC]]

---

## REST vs gRPC: когда что

| Критерий | REST (HTTP/JSON) | gRPC (Protobuf) |
|----------|------------------|-----------------|
| Клиент | Браузер, внешние API | Межсервисное взаимодействие |
| Формат | JSON (текст) | Protobuf (бинарный) |
| Контракт | OpenAPI (опционально) | `.proto` файлы |
| Streaming | Ограниченно | Встроен (Middle) |

**Правило:** внешний API — REST; внутренние вызовы между сервисами — gRPC.

Порты: HTTP `:8080`, gRPC `:9090`, metrics/health `:8081`.

---

## Protobuf: чтение `.proto`

```protobuf
syntax = "proto3";

package notes.v1;

service NoteService {
  rpc GetNote(GetNoteRequest) returns (GetNoteResponse);
}

message GetNoteRequest {
  int64 id = 1;
}

message GetNoteResponse {
  Note note = 1;
}

message Note {
  int64 id = 1;
  string title = 2;
}
```

- `message` — структура данных
- `service` + `rpc` — методы сервиса
- Числа полей (`= 1`) — теги для совместимости версий

Генерация Go-кода: `protoc` + плагины `protoc-gen-go`, `protoc-gen-go-grpc`.

---

## Unary RPC: сервер и клиент

```go
// Сервер
type server struct {
    pb.UnimplementedNoteServiceServer
    proc *processor.Notes
}

func (s *server) GetNote(ctx context.Context, req *pb.GetNoteRequest) (*pb.GetNoteResponse, error) {
    note, err := s.proc.GetByID(ctx, req.Id)
    if err != nil {
        return nil, status.Errorf(codes.NotFound, "note not found")
    }
    return &pb.GetNoteResponse{Note: toProto(note)}, nil
}

lis, _ := net.Listen("tcp", ":9090")
grpcServer := grpc.NewServer()
pb.RegisterNoteServiceServer(grpcServer, &server{proc: proc})
grpcServer.Serve(lis)
```

```go
// Клиент
conn, _ := grpc.Dial("localhost:9090", grpc.WithTransportCredentials(insecure.NewCredentials()))
client := pb.NewNoteServiceClient(conn)
resp, err := client.GetNote(ctx, &pb.GetNoteRequest{Id: 1})
```

---

## Context в gRPC

Как и в HTTP — `context.Context` первым аргументом. Используется для:
- таймаутов (`context.WithTimeout`)
- отмены при shutdown
- propagation trace ID (Middle)

---

## Общие protobuf-контракты

В продакшене `.proto` живут в общей библиотеке (в матрице — `git.indels.tech/Drivee/common`). Сервисы импортируют сгенерированный код, а не копируют `.proto`.

Junior: уметь прочитать `.proto`, понять message/service, вызвать unary RPC.

---

## Частые ошибки

| Ошибка | Правильно |
|--------|-----------|
| gRPC для публичного API из браузера | REST или gRPC-Web с прокси |
| Игнорировать `ctx` в RPC | Пробрасывать от HTTP handler или consumer |
| Не встраивать `UnimplementedXServer` | Нужно для forward compatibility |
| Один порт для HTTP и gRPC | Разные порты или grpc-gateway |

---

## Как проверить, что понял

- [ ] Прочитать `.proto` и назвать поля request/response
- [ ] Объяснить, почему payment-сервис зовёт другой сервис по gRPC, а не REST
- [ ] Написать unary handler, возвращающий `codes.NotFound`

**Связанные темы:** [[02-HTTP-REST-chi-Junior]] · [[06-Архитектура-Junior]]
