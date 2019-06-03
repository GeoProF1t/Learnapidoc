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

Документация Eventbrite создана на Apiary, который предоставляет функцию пробного запуска или [API Explorer](../Publishing-doc/Design-patterns.md#fifth) на правой панели. Панель открывается при нажатии на кнопку **Retrieve an Event**

![Retrieve an Event](img/1.png)

На панели консоли справа нажмите кнопку `Try`, чтобы включить возможность вызова ресурса (если кнопка `Call Resource` еще не отображается)

Перед успешным вызовом ресурса сделаем следующее:

- Перейдем на вкладку **URI Parameters** и изменим значение для идентификатора события на `49216045517`. Этот идентификатор соответствует проведенному семинару под названием «Документирование API-интерфейсов REST», который можно просмотреть [здесь](https://www.eventbrite.com/e/documenting-rest-apis-a-jumpstart-workshop-for-technical-writers-tickets-49216045517#). Идентификатор события будет отображаться в URL.
- Перейдем на вкладку **Headers** и добавим свой собственный токен OAuth в поле `PERSONAL_OAUTH_TOKEN`.

![Headers](img/2.png)

Нажмем кнопку `Call Resource` и затем прокрутим вниз до раздела **Body Response**, чтобы увидеть ответ. Он должен содержать основной текст страницы Eventbrite. (При ошибке с токеном OAuth или идентификатором события, отобразится ответ об ошибке.)

<a name="codeRequest"></a>
## 4. Получаем jQuery AJAX код запроса

Мы получили ответ. Теперь ради интереса выведем ответ на странице (как будто мы рекламируем это событие на нашем собственном сайте).

На панели консоли под разделом параметров кликнем **Show Code Example**

![Show Code Example](img/3.png)

в раскрывающемся списке языков выбираем параметр cURL и копируем значение. Это должно выглядеть примерно так:

```javascript
curl --include \
     --header "Authorization: Bearer IO6EB7MM6TSCIL2TIOHC" \
     --header "Content-Type: application/json" \
  'https://www.eventbriteapi.com/v3/events/49216045517/'
```

Открываем [Postman](../like-developer/submit-requests-postman.md) В Postman переходим **File > Import** и выбираем вкладку **Paste Raw Text**. В поле вставляем скопированный код curl.

![curl code](img/4.png)

После чего кликаем **Import**. Информация будет отображаться в новой вкладке Postman. В Postman, если развернуть вкладку **Headers**, вы увидите информацию OAuth. Поле GET включает извлеченную конечную точку Eventbrite с идентификатором события, добавленным в качестве [параметра пути](../documenting-api-endpoints/step3-parameters.md#pathParam).

В Postman, под кнопкой `Send`, нажимаем `Code`. В диалоговом окне **Generate Code Snippets** выбираем **JavaScript > Jquery AJAX**. Копируем полученное значение

![Copy value](img/5.png)

Удаляем пары ключ-значение `cache-control` и `Postman-Token` чтобы пример кода выглядел так:

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://www.eventbriteapi.com/v3/events/49216045517/",
  "method": "GET",
  "headers": {
    "Authorization": "Bearer IO6EB7MM6TSCIL2TIOHC",
    "Content-Type": "application/json"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

<a name="customize"></a>
## 5. Настраиваем ответ на странице

Информация, возвращаемая из объекта `event`, содержит гораздо больше деталей, чем нам нужно. Мы просто хотим отобразить название и описание мероприятия на нашем сайте. Для этого мы будем использовать код jQuery AJAX, скопированный с предыдущего шага. Более подробно об использовании AJAX в предыдущих разделах:

- [Изучение данных JSON ответа](../like-developer/inspect-json.md)
- [Доступ и вывод на страницу определенного значения JSON](../like-developer/access-print-value.md)
- [Точечная нотация](../like-developer/dot-notation.md)

Чтобы интегрировать код JQuery AJAX из Postman в свой веб-сайт и вывести поля `title`  и `description` в ответе, используем следующий код:

```html
<!DOCTYPE html>
<html>
<meta charset="UTF-8">

<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>

<script>
  var settings = {
    "async": true,
    "crossDomain": true,
    "url": "https://www.eventbriteapi.com/v3/events/49216045517/",
    "method": "GET",
    "headers": {
      "Authorization": "Bearer IO6EB7MM6TSCIL2TIOHC",
      "Content-Type": "application/json"
    }
  }

  $.ajax(settings).done(function (data) {
    console.log(data);
    var content = "<h2>" + data.name.text + "</h2>" + data.description.html;
    $("#eventbrite").append(content);
  });
</script>

<div id="eventbrite"></div>

</body>
</html>
```

Вот [результат](https://idratherbewriting.com/learnapidoc/assets/files/eventbrite-example.html)

![результат](img/6.png)

<a name="explanation"></a>
## Пояснения кода

Пример реализации настолько прост, насколько это возможно в отношении стиля. Но в примерах кода документации API всегда есть желание хранить простые примеры кода. На самом деле, скорее всего, демоверсия вообще не нужна. Просто отображение полезной нагрузки, возвращаемой в браузере, достаточно для разработчика пользовательского интерфейса. Однако для тестирования интересно, чтобы контент действительно отображался на странице.

Вкратце, вот что происходит. Метод `ajax` из jQuery получает полезную нагрузку для URL-адреса конечной точки, а затем назначает его аргументу `data`. При желании мы регистрируем `data` в консоли, чтобы легче было проверить их полезную нагрузку. Чтобы вывести различные свойства объекта, мы используем точечную нотацию. `data.name.text` получает свойство text из объекта `name`, а `data.description.html` получает тело.

Затем мы переименовываем содержимое, которое нам нужно, с помощью переменной (`var content`) и используем `append` метод jQuery, чтобы назначить содержимое переменной определенному тегу (`eventbrite`) на странице.

[🔙](RESTAPI-activities.md)

[Go next ➡](Retrieve-gallery-using-Flickr-API.md)
