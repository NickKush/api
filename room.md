API комнат
==========
- [**Основное**](#Основное)
    - [`POST` `P` `/api/room` Данные комнаты](#Данные-комнаты)
    - [`POST` `A` `/api/room/create` Создать комнату](#Создать-комнату)
    - [`POST` `P` `/api/room/getCurrentVideo` Текущая трансляция комнаты](#Текущая-трансляция-комнаты)
    - [`POST` `A` `/api/room/modify` Редактировать комнату](#Редактировать-комнату)
    - [`POST` `P` `/api/room/my` Список комнат пользователя](#Список-комнат-пользователя)
    - [`POST` `A` `/api/room/periscopeRandom` Ссылка на случайный стрим с перископа](#Ссылка-на-случайный-стрим-с-перископа)
- [**Плейлист**](#Плейлист)
    - [`POST` `P/A` `/api/room/playlist` Плейлист комнаты](#Плейлист-комнаты)
    - [`POST` `P/A` `/api/room/playlist/history` История воспроизведения комнаты](#История-воспроизведения-комнаты)
    - [`POST` `A` `/api/room/playlist/set` Изменить плейлист комнаты](#Изменить-плейлист-комнаты)
- [**Пользователи**](#Пользователи)
    - [`POST` `P` `/api/room/user/admin` Статус администратора комнаты](#Статус-администратора-комнаты)
    - [`POST` `A` `/api/room/user/list` Список имеющих доступ в комнату](#Список-имеющих-доступ-в-комнату)
    - [`POST` `A` `/api/room/user/remove` Лишить пользователя доступа в комнату](#Лишить-пользователя-доступа-в-комнату)
    - [`POST` `A` `/api/room/user/set` Изменить права доступа пользователя в комнату](#Изменить-права-доступа-пользователя-в-комнату)


## Основное

#### Данные комнаты
##### [`POST` `P` `/api/room`](https://funstream.tv/api/room)
**запрос**
```js
{
    roomId: <int> // Идентификатор комнаты
}
```
**ответ**
```js
{
    id: <int>, // Идентификатор комнаты
    name: <string>, // Имя комнаты
    owner: <obj>, // Создатель комнаты, объект из ответа /api/user
    category: <obj>, // Категория комнаты, объект из ответа /api/category
    mode: <string>, // Режим комнаты, [public | private]
    hidden: <bool>, // Флаг скрытия комнаты в фильтре
    rating: <int> // Рейтинг комнаты
}
```
*[`/api/user`](common.md#Данные-пользователя), [`/api/category`](common.md#Категория-контента)*  
*Вернёт ошибку если передан неверный идентификатор или комната в приватном режиме и текущий пользователь не имеет в неё доступа.*


#### Создать комнату
##### [`POST` `A` `/api/room/create`](https://funstream.tv/api/room/create)
**запрос**
```js
{
    name: <string>, // Имя комнаты
    categoryId: <int>, // Идентификатор категории
    mode: <string>, // Режим комнаты, [public | private], по умолчанию public
    hidden: <bool>, // Флаг скрытия комнаты в фильтре, по умолчанию false
}
```
**ответ**
```js
{
    // Данные комнаты, объект из ответа /api/room
}
```
*[`/api/room`](#Данные-комнаты)*  
*Вернёт ошибкку если у пользователя уже создано 3 комнаты, передан неверный идентификатор категории или неверный режим.*


#### Текущая трансляция комнаты
##### [`POST` `P` `/api/room/getCurrentVideo`](https://funstream.tv/api/room/getCurrentVideo)
**запрос**
```js
{
    roomId: <int> // Идентификатор комнаты
}
```
**ответ**
```js
{
    roomId: <number>, // Идентификатор комнаты
    url: <string>, // Ссылка на трансляцию
    code: { // HTML код плеера трансляции
        started: <string>, // С автозапуском
        paused: <string> // Без автозапуска
    }
}
```
*Не может быть использован в [/api/bulk](common.md#Пакетный-запрос) запросе*  
*Вернёт ошибку если комната приватная и текущий пользователь не имеет в неё доступа.*


#### Редактировать комнату
##### [`POST` `A` `/api/room/modify`](https://funstream.tv/api/room/modify)
**запрос**
```js
{
    roomId: <int>, // Идентификатор комнаты
    ... // Поля из запроса /api/room/create
}
```
**ответ**
```js
{
    // Данные комнаты, объект из ответа /api/room
}
```
*[`/api/room/create`](#Создать-комнату), [`/api/room`](#Данные-комнаты)*  
*Вернёт ошибкку если передан неверный идентификатор комнаты неверный идентификатор категории или неверный режим.*


#### Список комнат пользователя
##### [`POST` `A` `/api/room/my`](https://funstream.tv/api/room/my)
**запрос**
```js
{}
```
**ответ**
```js
[
    {
        room: <obj>, // Данные комнаты, объект из ответа /api/room
        type: <string> // Тип прав доступа в комнату, [user | admin]
    },
    ...
]
```
*[`/api/room`](#Данные-комнаты)*  
*Список комнат, доступ в которые имеет пользователь или является в них администратором.*


#### Ссылка на случайный стрим с перископа
##### [`POST` `A` `/api/room/periscopeRandom`](https://funstream.tv/api/room/periscopeRandom)
**запрос**
```js
{}
```
**ответ**
```js
{
    url: <string> // Ссылка на онлайн стрим перископа
}
```


## Плейлист

#### Плейлист комнаты
##### [`POST` `P/A` `/api/room/playlist`](https://funstream.tv/api/room/playlist)
**запрос**
```js
{
    roomId: <int> // Идентификатор комнаты
}
```
**ответ**
```js
[
    {
      id: <int>, // Идентификатор записи в плейлисте
      position: <int>, // Позиция в плейлисте
      url: <string>, // Ссылка на стрим/видео
      start: <int>, // Время начала воспроизведения, unixtime
      main: <bool> // Флаг основного стрима
    },
    ...
]
```
*Позицию воспринимать как ключ для сортировки, значения могут идти с разным интервалом.*  
*Вернёт ошибку если комната приватная и текущий пользователь не имеет в неё доступа.*


#### История воспроизведения комнаты
##### [`POST` `P/A` `/api/room/playlist/history`](https://funstream.tv/api/room/playlist/history)
**запрос**
```js
{
    roomId: <int> // Идентификатор комнаты
}
```
**ответ**
```js
[
    {
        url: <string>, // Ссылка на стрим/видео
        start: <int>, // Время начала воспроизведения, unixtime
        end: <int>, // Время окончания воспроизведения, unixtime
    },
    ...
]
```
*Вернёт ошибку если комната приватная и текущий пользователь не имеет в неё доступа.*


#### Изменить плейлист комнаты
##### [`POST` `A` `/api/room/playlist/set`](https://funstream.tv/api/room/playlist/set)
**запрос**
```js
{
    roomId: <int>, // Идентификатор комнаты
    playlist: [ // Новый плейлист
        {
            position: <int>, // Позиция в плейлисте
            url: <string>, // Ссылка на стрим/видео
            start: <int>, // Время начала воспроизведения, unixtime
            main: <bool> // Флаг основного стрима
        },
        ...
    ]
}
```
**ответ**
```js
{}
```
*Вернёт ошибку если указан неверный идентификатор комнаты, текущий пользователь не имеет прав администратора указаннойкомнаты, для любого элемента плейлиста не указано любое из полей, указано более одного основного стрима.*


## Пользователи

#### Статус администратора комнаты
##### [`POST` `P` `/api/room/user/admin`](https://funstream.tv/api/room/user/admin)
**запрос**
```js
{
    roomId: <int> // Идентификатор комнаты
}
```
**ответ**
```js
{
    admin: <bool> // Статус администратора комнаты
}
```
*Вернёт ошибку если указан неверный идентификатор комнаты.*


#### Список имеющих доступ в комнату
##### [`POST` `A` `/api/room/user/list`](https://funstream.tv/api/room/user/list)
**запрос**
```js
{
    roomId: <int> // Идентификатор комнаты
}
```
**ответ**
```js
[
    {
        type: <string>, // Тип прав доступа в комнату, [user | admin]
        user: <obj> // Данные пользователя, объект из ответа /api/user
    },
    ...
]
```
*[`/api/user`](common.md#Данные-пользователя)*  
*Вернёт ошибку если указан неверный идентификатор комнаты или текущий пользователь не имеет прав администратора комнаты.*


#### Лишить пользователя доступа в комнату
##### [`POST` `A` `/api/room/user/remove`](https://funstream.tv/api/room/user/remove)
**запрос**
```js
{
    roomId: <int>, // Идентификатор комнаты
    userId: <int> // Идентификатор пользователя
}
```
**ответ**
```js
{}
```
*Вернёт ошибку если указан неверный идентификатор комнаты или текущий пользователь не имеет прав администратора комнаты.*


#### Изменить права доступа пользователя в комнату
##### [`POST` `A` `/api/room/user/set`](https://funstream.tv/api/room/user/set)
**запрос**
```js
{
    roomId: <int>, // Идентификатор комнаты
    userId: <int>, // Идентификатор пользователя
    type: <string>, // Тип прав доступа в комнату, [user | admin]
}
```
**ответ**
```js
{}
```
*Значение поля `type` указывает на новые права пользователя. Т.е. если было `admin`, а стало `user`, то это лишение прав администратора комнаты.*  
*Вернёт ошибку если указан неверный идентификатор комнаты или текущий пользователь не имеет прав администратора комнаты.*