# [Peka2.tv](http://peka2.tv) API

## Текущая версия
### 0.X.X
### [История изменений](CHANGELOG.md)


## Общее

Запрос посылается методом `POST` для основного апи и через `Websocket` для чата, если не указано иного. Параметры запроса в JSON формате, **протокол HTTP**.  
Авторизация происходит через токен в `Header`. Например
```
POST /api/user/current HTTP/1.1
Token: Bearer your-token-here
```

Успешный ответ приходит со статусом `200`.  
При ошибке ответ приходит со статусом `500`. Формат ответа ошибки
```ts
{
    message: string; // Текст ошибки
}
```

Для описания параметров используется формат TypeScript интерфейсов и типов.  
Параметры отмеченные `?` являются необязательными.  
Где написано ``объект из ответа ...``, если не указано иного, подразумевает ответ указанного запроса без необязательных параметров/опций.

Версия API передается через `Accept`. Например,
```
POST /user/current HTTP/1.1
Accept: application/json; version=1.0
```
*В данный момент передавать версию не обязательно*

Запросы передаются на сайт [`http://funstream.tv`](http://funstream.tv) для общего API и на [`wss://chat.funstream.tv`](wss://chat.funstream.tv) для чата.

Примеры запросов на `curl`
```sh
curl -H "Content-Type: application/json" -H "Accept: application/json; version 1.0" -X POST http://funstream.tv/api/user/current
curl -H "Content-Type: application/json" -H "Accept: application/json; version 1.0" -H "Token: Bearer ..." -X POST -d '{content: "stream"}' http://funstream.tv/api/subscribe/subscribers
```


##### В случае вопросов, ошибок или неточностей документации, пишите в Помощь на сайте [funstream.tv](http://funstream.tv/stream/all/top) (необходимо залогиниться, категория 'Технические вопросы') или в Мейн чат на сайте [peka2.tv](http://peka2.tv) пользователю `drow`.


# API

#### Права доступа
- **`P`** Публичный, авторизация не обязательна
- **`A`** Все авторизованные пользователи
- **`M`** Модераторы, роль `moderator`
- **`S`** Саппорты, роль `support`
- **`B`** Администратор блокировок, роль `blocker`
- **`Sm`** Администратор смайлов, роль `smiler`
- **`MS`** Стример с партнёркой, роль `masterstreamer`
- **`RA`** Администратор ролей, роль `roleAdmin`
- **`StA`** Администратор магазина, роль `storeAdmin`
- **`StS`** Поддержка магазина, роль `storeSupport`
- **`C`** Закрытый, для внутреннего использования

---


- [**OAuth**](oauth.md)
    - [`POST` `A` `/api/oauth/app` Получить данные приложения](oauth.md#Получить-данные-приложения)
    - [`POST` `A` `/api/oauth/app/set` Сохранить данные приложения](oauth.md#Сохранить-данные-приложения)
    - [`POST` `P` `/api/oauth/check` Проверка статуса кода](oauth.md#Проверка-статуса-кода)
    - [`POST` `P` `/api/oauth/exchange` Получить токен по коду](oauth.md#Получить-токен-по-коду)
    - [`POST` `A` `/api/oauth/grant` Предоставить доступ по коду](oauth.md#Предоставить-доступ-по-коду)
    - [`POST` `P` `/api/oauth/request` Запросить код](oauth.md#Запросить-код)
- [**Чат**](chat.md)
    - [**Протокол взаимодействия**](chat.md#Протокол-взаимодействия)
        - [Примеры взаимодействия для разных языков](chat-client-examples.md)
    - [**Оповещение сервера**](chat.md#Оповещение-сервера)
        - [`WS` `P` `/chat/login` Подписаться на события пользователя](chat.md#Подписаться-на-события-пользователя)
        - [`WS` `A` `/chat/logout` Отписаться от событий пользователя](chat.md#Отписаться-от-событий-пользователя)
        - [`WS` `P` `/chat/join` Присоединится к каналу](chat.md#Присоединится-к-каналу)
        - [`WS` `P` `/chat/leave` Покинуть канал](chat.md#Покинуть-канал)
        - [`WS` `P` `/chat/history` История канала](chat.md#История-канала)
        - [`WS` `A` `/chat/publish` Отправить сообщение](chat.md#Отправить-сообщение)
        - [`WS` `P` `/chat/channel/list` Список пользователей в канале](chat.md#Список-пользователей-в-канале)
    - [**Оповещение клиента**](chat.md#Оповещение-клиента)
        - [`WS` `P` `/chat/message` Новое сообщение](chat.md#Новое-сообщение)
        - [`WS` `P` `/chat/message/remove` Удаление сообщения](chat.md#Удаление-сообщения)
    - [**Каналы чата**](chat.md#Каналы-чата)
    - [**Типы сообщений**](chat.md#Типы-сообщений)
- [**Общее**](common.md)
    - [**Пользователь**](common.md#Пользователь)
        - [`POST` `P` `/api/user` Данные пользователя](common.md#Данные-пользователя)
        - [`POST` `P` `/api/user/list` Данные списка пользователей](common.md#Данные-списка-пользователей)
        - [`POST` `P` `/api/user/current` Данные текущего пользователя](common.md#Данные-текущего-пользователя)
        - [`POST` `RA` `/api/user/full` Полные данные пользователя](common.md#Полные-данные-пользователя)
        - [`POST` `C` `/api/user/login` Логин](common.md#Логин)
        - [`POST` `P` `/api/user/login/bytoken` Логин по токену](common.md#Логин-по-токену)
        - [`POST` `P` `/api/user/logout` Логаут](common.md#Логаут)
        - [`POST` `A` `/api/user/settings` Получить или установить настройки текущего пользователя](common.md#Получить-или-установить-настройки-текущего-пользователя)
        - [`POST` `P` `/api/user/forgot` Запрос на сброс пароля](common.md#Запрос-на-сброс-пароля)
        - [`POST` `P` `/api/user/restore` Установка пароля](common.md#Установка-пароля)
        - [`POST` `RA` `/api/user/roles/list` Список пользователей с ролью](common.md#Список-пользователей-с-ролью)
        - [`POST` `RA` `/api/user/roles/set` Изменить роль пользователя](common.md#Изменить-роль-пользователя)
    - [**Категория**](common.md#Категория)
        - [`POST` `P` `/api/category` Категория контента](common.md#Категория-контента)
    - [**Стрим**](common.md#Стрим)
        - [`POST` `P` `/api/stream` Данные стрима](common.md#Данные-стрима)
    - [**Чат**](common.md#Чат)
        - [`POST` `P` `/api/channel/data` Данные каналов](common.md#Данные-каналов)
        - [`POST` `P` `/api/channel/private` Список приват каналов](common.md#Список-приват-каналов)
    - [**Фильтр**](common.md#Фильтр)
        - [`POST` `A/P` `/api/content` Список элементов контента](common.md#Список-элементов-контента)
        - [`POST` `P` `/api/content/top` Топ N элементов контента](common.md#Топ-n-элементов-контента)
    - [**Подписки**](common.md#Подписки)
        - [`POST` `A` `/api/subscribe/add` Подписаться](common.md#Подписаться)
        - [`POST` `A` `/api/subscribe/amount` Количество активных подписок онлайн](common.md#Количество-активных-подписок)
        - [`POST` `A` `/api/subscribe/check` Проверить подписку](common.md#Проверить-подписку)
        - [`POST` `A` `/api/subscribe/list` Список подписок](common.md#Список-подписок)
        - [`POST` `A` `/api/subscribe/remove` Отписаться](common.md#Отписаться)
        - [`POST` `A` `/api/subscribe/subscribers` Список подписчиков пользователя](common.md#Список-подписчиков-пользователя)
    - [**Игноры**](common.md#Игноры)
        - [`POST` `A` `/api/ignore/add` Добавить в список игнорируемых](common.md#Добавить-в-список-игнорируемых)
        - [`POST` `A` `/api/ignore/check` Проверить на игнор](common.md#Проверить-на-игнор)
        - [`POST` `A` `/api/ignore/list` Список игнорируемого](common.md#Список-игнорируемого)
        - [`POST` `A` `/api/ignore/remove` Удалить из списка игнорируемого](common.md#Удалить-из-списка-игнорируемого)
    - [**Дополнительные вызовы**](common.md#Дополнительные-вызовы)
        - [`POST` `P` `/api/bulk` Пакетный запрос](common.md#Пакетный-запрос)
- [**Смайлы**](smile.md)
    - [**Смайлы**](smile.md#Смайлы)
        - [`POST` `P` `/api/smile` Доступные смайлы](smile.md#Доступные-смайлы)
        - [`POST` `MS/Sm` `/api/smile/add` Добавить смайл](smile.md#Добавить-смайл)
        - [`POST` `Sm` `/api/smile/approve` Утвердить изменения в смайлах стримера](smile.md#Утвердить-изменения-в-смайлах-стримера)
        - [`POST` `Sm` `/api/smile/pending` Список изменений в смайлах стримеров на утверждение](smile.md#Список-изменений-в-смайлах-стримеров-на-утверждение)
        - [`POST` `Sm` `/api/smile/reject` Отклонить изменения в смайлах стримера](smile.md#Отклонить-изменения-в-смайлах-стримера)
        - [`POST` `MS/Sm` `/api/smile/remove` Удалить смайлы](smile.md#Удалить-смайлы)
        - [`POST` `MS/Sm` `/api/smile/update` Обновить смайлы](smile.md#Обновить-смайлы)
        - [`POST` `MS` `/api/smile/ms/pending` Наличие смайлов в ожидании утверждения](smile.md#Наличие-смайлов-в-ожидании-утверждения)
        - [`POST` `MS` `/api/smile/ms/prepare` Отправить текущие изменения в смайлах на утверждение](smile.md#Отправить-текущие-изменения-в-смайлах-на-утверждение)
        - [`POST` `MS` `/api/smile/ms/revert` Откатить изменения в смайлах](smile.md#Откатить-изменения-в-смайлах)
    - [**Иконки**](smile.md#Иконки)
        - [`POST` `Ms` `/api/icon/add` Добавление иконки](smile.md#Добавление-иконки)
        - [`POST` `P` `/api/icon/list` Список иконок](smile.md#Список-иконок)
        - [`POST` `Ms` `/api/icon/remove` Удаление иконок](smile.md#Удаление-иконок)
- [**Магазин**](store.md)
    - [**Бонусы**](store.md#Бонусы)
        - [Типы бонусов и их параметры](store.md#Типы-бонусов-и-их-параметры)
        - [`POST` `StA` `/api/store/bonus/modify` Создание/изменение бонуса](store.md#Созданиеизменение-бонуса)
        - [`POST` `StA` `/api/store/bonus/list` Список бонусов](store.md#Список-бонусов)
    - [**Бонусы пользователя**](store.md#Бонусы-пользователя)
        - [`POST` `StS` `/api/store/purchase/list` Список бонусов пользователя](store.md#Список-бонусов-пользователя)
        - [`POST` `A` `/api/store/purchase/my` Список бонусов текущего пользователя](store.md#Список-бонусов-текущего-пользователя)
        - [`POST` `StS` `/api/store/purchase/remove` Удалить бонус пользователя](store.md#Удалить-бонус-пользователя)
        - [`POST` `StS` `/api/store/purchase/modify` Изменение данных бонуса пользователя](store.md#Изменение-данных-бонуса-пользователя)
        - [`POST` `A/StS` `/api/store/purchase/setStatus` Изменение статуса бонуса пользователя](store.md#Изменение-статуса-бонуса-пользователя)
    - [**Покупка бонуса**](store.md#Покупка-бонуса)
        - [`POST` `A` `/api/store/buy/pekoins` Покупка бонуса за пекоины](store.md#Покупка-бонуса-за-пекоины)
        - [`POST` `A` `/api/store/buy/points` Покупка бонуса за баллы](store.md#Покупка-бонуса-за-баллы)
    - [**Баллы**](store.md#Баллы)
        - [`POST` `StS` `/api/store/points/get` Баллы пользователя](store.md#Баллы-пользователя)
        - [`POST` `A` `/api/store/points/my` Баллы текущего пользователя](store.md#Баллы-текущего-пользователя)
        - [`POST` `StS` `/api/store/points/set` Изменить баллы пользователя](store.md#Изменить-баллы-пользователя)
    - [**Подписки на стримеров**](store.md#Подписки-на-стримеров)
        - [`POST` `A` `/api/store/subscription/purchase` Купить подписку на стримера](store.md#Купить-подписку-на-стримера)
        - [`POST` `StS` `/api/store/subscription/list` Список подписок пользователя](store.md#Список-подписок-пользователя)
        - [`POST` `A` `/api/store/subscription/my` Список подписок текущего пользователя](store.md#Список-подписок-текущего-пользователя)
        - [`POST` `StS` `/api/store/subscription/modify` Добавление/изменение подписки пользователя](store.md#Добавлениеизменение-подписки-пользователя)
        - [`POST` `StS` `/api/store/subscription/remove` Удаление подписки пользователя](store.md#Удаление-подписки-пользователя)
        - [`POST` `A/StS` `/api/store/subscription/setStatus` Изменение статуса подписки](store.md#Изменение-статуса-подписки)
    - [**Уведомления**](store.md#Уведомления-1)
        - [`bonus/purchase` Куплен/добавлен бонус](store.md#Куплендобавлен-бонус)
        - [`bonus/points` Изменился баланс баллов](store.md#Изменился-баланс-баллов)
        - [`bonus/subscription` Изменилась/добавилась подписка](store.md#Измениласьдобавилась-подписка)
- [**Платежи**](payment.md)
    - [**Уведомления**](payment.md#Уведомления-1)
        - [`payment/stream` Донат на стрим](payment.md#Донат-на-стрим)
- [**Плеер**](player.md)
    - [`GET` `P` `/api/player/live` Онлайн статус трансляций](player.md#Онлайн-статус-трансляций)
- [**Админка**](admin.md)
    - [**Модерация**](admin.md#Модерация)
        - [`POST` `A` `/api/moderation/accuse` Забанить пользователя](admin.md#Забанить-пользователя)
        - [`POST` `B` `/api/moderation/block` Блокировка пользователя](admin.md#Блокировка-пользователя)
        - [`POST` `P` `/api/moderation/check` Проверить забанен ли пользователь](admin.md#Проверить-забанен-ли-пользователь)
        - [`POST` `A` `/api/moderation/list` Получить список банов](admin.md#Получить-список-банов)
        - [`POST` `P` `/api/moderation/reasons` Получить список причин бана](admin.md#Получить-список-причин-бана)
        - [`POST` `M` `/api/moderation/undo` Отменить бан](admin.md#Отменить-бан)
    - [**Поддержка**](admin.md#Поддержка)
        - [`POST` `A` `/api/support/ask` Задать вопрос](admin.md#Задать-вопрос)
        - [`POST` `A` `/api/support/ban` Отключить возможность задавать вопросы](admin.md#Отключить-возможность-задавать-вопросы)
        - [`POST` `A` `/api/support/channels` Список последних вопросов](admin.md#Список-последних-вопросов)
        - [`POST` `S` `/api/support/list` Получить список вопросов](admin.md#Получить-список-вопросов)
    - [**Безопасность**](admin.md#Безопасность)
        - [`POST` `A` `/api/security/user` Список последних логинов](admin.md#Список-последних-логинов)
    - [**Уведомления**](admin.md#Уведомления-1)
        - [`chatBan` Бан в чате](admin.md#Бан-в-чате)
        - [`chatBanAttempt` Отметка о нарушении в чате](admin.md#Отметка-о-нарушении-в-чате)
        - [`chatBanUndo` Отмена бана в чате](admin.md#Отмена-бана-в-чате)
- [**Уведомления**](notification.md)
    - [**Событие уведомления**](notification.md#Событие-уведомления)
    - [**Прочие уведомления**](notification.md#Прочие-уведомления)
        - [**Сторонние сервисы в чате**](notification.md#Сторонние-сервисы-в-чате)
            - [`chatThirdpartySendMessageError` Ошибка отправки сообщения в чат сервис](notification.md#Ошибка-отправки-сообщения-в-чат-сервис)
- [**Сторонние сервисы**](thirdparty.md)
    - [`POST` `C` `/api/oauth/thirdparty/register` Регистрация через сторонние сервисы](thirdparty.md#Регистрация-через-сторонние-сервисы)
    - [**Уведомления**](thirdparty.md#Уведомления-1)
        - [`thirdpartyLogin` Логин через сторонние сервисы](thirdparty.md#Логин-через-сторонние-сервисы)
        - [`thirdpartyRegister` Регистрация через сторонние сервисы](thirdparty.md#Регистрация-через-сторонние-сервисы-1)


Спасибо @JAremko за помощь в оформлении документации.
