# Hydra API

NodeJS библиотека для работы с API сервиса "Hydra Coin"

# Установка
**npm**
 `npm i hydracoinapi`

## Подключение

``` js
const {
    HydraApi
} = require('hydracoinapi')
const hapi = new HydraApi('Token', 'user_id')
```

## Методы API

***call*** - универсальный метод отправки запроса

| Параметр | Тип | Обязателен | Описание |
|--|--|--|--|
| methodName | string | Да |Имя метода |
| params | object | Нет | Параметры запроса |

##
***getProjectInfo*** - Получить информацию о Вашем проекте

**Пример:**

``` js
async function run() {
    const info = await ds.getProjectInfo()
    console.log(info)
}
run().catch(console.error);
```

##
***editProjectInfo*** - Редактировать информацию о Вашем проекте

| Параметр | Тип | Обязателен | Описание |
|--|--|--|--|
| name | string | Нет | Название проекта |
| avatar| string | Нет | Прямая ссылка на новый аватар проекта |
| group_id| number | Нет | ID группы проекта |

* Все параметры должны быть переданы 

**Пример:**

``` js
async function run() {
    const info = await hapi.editProjectInfo(
        'My app',
        'vk.com/images/camera_200.png',
        1
    );
    console.log(info)
};

run().catch(console.error);
```

##
***sendPayment*** - Совершить перевод монет указанному пользователю

| Параметр | Тип | Обязателен | Описание |
|--|--|--|--|
| toId| number | да| ID пользователя, которому Вы собираетесь совершить перевод |
| amount | number | да|Количество монет, которое Вы собираетесь перевести указанному пользователю  |

**Пример:**

``` js
async function run() {
    const info = await hapi.sendPayment(1, 1);
    console.log(info);
};

run().catch(console.error);
```

##
***getPaymentLink*** - Получить ссылку на перевод монет проекту

**Пример:**

``` js
async function run() {
    const info = await ds.getPaymentLink();
    console.log(info);
};

run().catch(console.error);
```

##
***Прослушивание входящих переводов:***

> *Наша библиотека автоматически сверяет hash входящих переводов, защищая Вас от злоумышленников.*

Для начала Вам стоит вызвать функцию **start**

| Параметр | Тип | Обязателен | Описание |
|--|--|--|--|
| path| string\number  | да | Ваш IP адрес или домен |
| port | number | нет |Прослушиваемый порт |

Затем Вам нужно подписаться на входящие переводы, используя функцию **onPayment**, в параметры который нужно передать callback функцию.

**Пример:**

``` js
function run() {
    ds.start('http://82.112.51.71', 80);

    ds.onPayment(context => {
        const {
            amount,
            fromId
        } = context;
        console.log(context);
    });
};

run().catch(console.error);
```
