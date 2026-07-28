
Ниже — **документация по событию AppsFlyer `myaccount_sum_done_view`** в контексте вашего фрагмента и бэкенда indelssite.

---

## Назначение

**Имя события в AF:** `myaccount_sum_done_view` (константа `User::APPSFLYER_EVENT_MYACCOUNT_SUM_DONE_VIEW`).

**Смысл (из словаря событий в коде):** показ экрана **результата** пополнения (успех / отказ / другое состояние платежа) в веб-чекауте мобильного кабинета.

Это **заключительный шаг** цепочки пополнения рядом с событиями:

- `myaccount_balance_topup_click` — нажали «Пополнить»
- `myaccount_method_list_view` — экран выбора метода
- `myaccount_sum_view` — экран суммы
- `myaccount_sum_done_redirect` — уход на провайдера
- **`myaccount_sum_done_view`** — возврат и показ результата

---

## Клиент (WebView / `checkout.php`)

1. После готовности DOM вызывается `notifyAppsFlyerMyaccountSumDoneView()` (через `runWhenDomReady`).
2. Выполняется **POST** на маршрут Yii:  
   **`/mobile/payment/appsflyerMyaccountSumDoneView`**
3. В **query string** передаются **`phone`** и **`token`** пользователя (как в открытой странице чекаута).
4. В **теле запроса** (form-data / стандартный POST от jQuery) уходят поля:
   - **`payment_method`** — строка: имя вендора платежа (`$payment->vendor->name`), либо пустая строка, если вендора нет.
   - **`error`** — текст причины отказа, если `$payment->status == Payment::STATUS_REJECT`, иначе пустая строка.

Ошибки AJAX **намеренно глушатся** (без toastr), чтобы не ломать UX.

---

## Сервер: контроллер

`PaymentController::actionAppsflyerMyaccountSumDoneView`:

- Разрешён только **POST**; иначе ответ с ошибкой (`only post`).
- Вызывается `$this->user->appsFlyerEventHandle(User::APPSFLYER_EVENT_MYACCOUNT_SUM_DONE_VIEW)`.
- При успехе — типичный JSON-ответ «Ok» через `sendOkResponse`.

Пользователь берётся из **`Yii::app()->user->getModel()`** (мобильный контроллер, сессия/логин), как и для остальных экшенов модуля `mobile`.

---

## Сервер: сбор payload для AF (`User::appsFlyerEventHandle`)

Для `myaccount_sum_done_view` в **`eventValue`** попадают:

| Ключ в `Appsflyer` | Источник |
|--------------------|----------|
| `customer_user_id` | `$this->id` |
| `eventTime` | UTC, `gmdate('Y-m-d H:i:s.000')` |
| `appsflyer_id` | `$this->appsflyer_id` |
| `app_version` | `$this->appversion` |
| `platform` | `$this->getDeviceOs()` |
| `city_id` | `$this->city_id` |
| `payment_method` | `$_POST['payment_method']` |
| `error` | `$_POST['error']` |

Плюс базовый **`af_city`** из `$this->city_id` (как для других событий в этом хендлере).

---

## Доставка в AppsFlyer (важно)

Класс **`Appsflyer`** не дергает HTTP API AppsFlyer напрямую из этого запроса. Метод **`saveEventToRedis()`** кладёт JSON в **Redis-список** с ключом **`appsflyer_events_to_send`**.

Событие **фактически не попадёт в очередь**, если не выполнены условия в `saveEventToRedis` (в т.ч. пустые **`appsflyer_id`**, **`eventName`**, **`deviceId`** / idfa — см. проверку в начале метода). У **`Appsflyer::create($user)`** при пустом `getDeviceOs()` возвращается неполностью заполненный объект — это тоже влияет на цепочку.

То есть документация для интеграции: **PHP фиксирует событие в Redis → дальше его забирает и отправляет в AF отдельный воркер/сервис** (его код в этом репозитории нужно искать отдельно по потребителю `appsflyer_events_to_send`).

---

## Как добавить **новое** событие AF по тому же паттерну

1. **Константа** имени события и запись в **`User::$appsflyerEvents`** (описание для внутреннего словаря).
2. В **`appsFlyerEventHandle`** — ветка `if ($eventName == self::APPSFLYER_EVENT_...)` с нужными полями в `$eventValue` (при необходимости новые ключи в классе **`Appsflyer`** и, если нужна строгая типизация в Go-консьюмере, — в **`APPSFLYER_GO_EVENT_VALUE_TYPES`**).
3. **Экшен** в `PaymentController` (или другом контроллере): POST → `appsFlyerEventHandle(...)`.
4. На **клиенте** — вызов POST с теми же `phone`/`token`, что и для авторизованной веб-сессии, и телом с полями, которые читает `$_POST` в п.2.

---
Exemple: `www/protected/modules/mobile/views/payment/checkout.php`
```php
function notifyAppsFlyerMyaccountSumDoneView() {

const payload = {

	payment_method: <?= json_encode($payment->vendor ? $payment->vendor->name : '') ?>,
	
	error: '<?= $payment->status == Payment::STATUS_REJECT ? $payment->reason : '' ?>',
	
	};
	
	$.ajax({
	
	url: '/mobile/payment/appsflyerMyaccountSumDoneView?phone=<?= rawurlencode($this->user->phone) ?>&token=<?= rawurlencode($this->user->token) ?>',
	
	type: 'POST',
	
	data: payload,
	
	success: function () { /* тихий успех */ },
	
	error: function () { /* не показывать toastr, чтобы не мешать UX */ }
	
	});

}
```