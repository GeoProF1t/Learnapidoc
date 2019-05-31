# 👨‍💻 Практическое занятие: Получить информацию о событии, используя API сервиса Eventbrite

Для получения заголовка и описания события будем использовать [API сервиса Eventbrite](https://www.eventbrite.com/platform/api#/introduction/quick-start)

[О Eventbrite](#about)

[1. Получаем токен OAuth](#getToken)

[2. Определяем необходимый ресурс и конечную точку](#determine)

[3. Создаем запрос](#request)

[4. Получаем jQuery AJAX код запроса](#codeRequest)

[5. Настраиваем ответ на странице](#customize)

[Пояснения кода](#explanation)

<a name="about"></a>
## О Eventbrite

Eventbrite - это инструмент управления событиями, с которым можно взаимодействовать через API для получения необходимой информации о событии. В этом примере используем API-интерфейс Eventbrite для отображения описания события на своей странице.

<a name="getToken"></a>
## 1. Получаем токен OAuth

Eventbrite использует [метод OAuth для авторизации](../conceptual-topics/authentication-and-authorization.md#auth2). Для выполнения любых запросов нам понадобится токен OAuth, о котором можно узнать в [документации по аутентификации на Eventbrite](https://www.eventbrite.com/platform/api).


Если есть желание подписаться на собственный токен, сначала нужно [войти в Eventbrite](https://www.eventbrite.com/), а затем создать и зарегистрировать свое приложение [здесь](https://www.eventbrite.com/signin/?referrer=%2Fmyaccount%2Fapps%2F). После создания приложения нужно нажать **Show Client Secret and OAuth Token** и скопировать «Anonymous access OAuth token».

<a name="determine"></a>
## 2. Определяем необходимый ресурс и конечную точку

Документация по Eventbrite API доступна по адресу https://www.eventbrite.com/platform/api/. Посмотреть список доступных конечных точек можно в разделе "Reference" на боковой панели. Какую конечную точку нам использовать?

Чтобы получить информацию о событии, мы будем использовать объект ["event"](https://www.eventbrite.com/platform/api#/reference/event), который представляет событие "Eventbrite". (API Eventbrite использует термин «объекты» вместо «ресурсы» ) В частности, мы будем использовать ["Retrieve"](https://www.eventbrite.com/platform/api#/reference/event). Единственный параметр, который нам нужно передать - это идентификатор события.

<a name="request"></a>
## 3. Создаем запрос

Хотя у Eventbrite есть пояснения, как передавать авторизацию в запросы, проще использовать предварительно созданный curl-запрос из примеров кода в документации, а затем использовать Postman для преобразования его в JavaScript jQuery AJAX.

Документация Eventbrite создана на Apiary, который предоставляет функцию пробного запуска или [API Explorer](../Publishing-doc/Design-patterns.md#fifth) на правой панели. Панель открывается при нажатии на кнопку **Retrieve an Event**:

![Retrieve an Event](img/1.png)

На панели консоли справа нажмите кнопку `Try`, чтобы включить возможность вызова ресурса (если кнопка `Call Resource` еще не отображается)

<a name="codeRequest"></a>
## 4. Получаем jQuery AJAX код запроса

<a name="customize"></a>
## 5. Настраиваем ответ на странице

<a name="explanation"></a>
## Пояснения кода
