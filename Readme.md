## Web Cache Deception and Web Cache Poisoning?

Перший допомагаэ получити дані користувача другий заставити косриутвача прочитати наші данні.

## Два параметра для проведення Session Fixation

Наприклад ми можемо відправити https://example.com/login;jsessionid=abcd1234 жертві важно чтоб jsessionid записався
сессію(xss наприклад) і друге чтоб не змінився

## Base64 and Base64URL encoding

Другий варінт більш підходе для урл, томущо замніє  "-"  на  "+", і a "_"  на "/"

## П'ять (або більше) типів Cross-Site Scripting (XSS):

1. **Stored XSS**: шкідливий скрипт зберігається на сервері (наприклад, у базі даних) і виконується при відвідуванні
   сторінки іншими користувачами.
    - **Приклад**: скрипт вставляється в коментар на форумі.

2. **Reflected XSS**: скрипт передається через URL або запит і відображається на сторінці без збереження.
    - **Приклад**: передача скрипта через параметр в URL (`https://example.com/?search=<script>alert(1)</script>`).

3. **DOM-Based XSS**: уразливість виникає через маніпуляції з об'єктною моделлю документа (DOM) на стороні клієнта.
    - **Приклад**: JavaScript на клієнтській стороні бере дані з URL і додає їх у HTML без фільтрації.

4. **Self XSS**: користувач сам виконує скрипт у консолі браузера, зазвичай через соціальну інженерію.
    - **Приклад**: атака через переконання жертви ввести скрипт у консолі для отримання «бонусу».

5. **Blind XSS**: атакуючий не бачить результат виконання скрипта, і він активується у сторонній системі, наприклад, в
   панелі адміністратора.
    - **Приклад**: скрипт надсилається через форму зворотного зв'язку і виконується, коли адміністратор переглядає
      повідомлення.

6. **Mutated XSS (або M-XSS)**: уразливість виникає при зміні браузером вихідного шкідливого коду під час рендерингу, що
   призводить до несподіваного виконання.
    - **Приклад**: браузер змінює вихідний код, і скрипт спрацьовує, хоча він був змінений і не відразу помітний.

## Як працює Boolean *Error* Inferential (Blind) SQL Injection

**Boolean Error Inferential SQL Injection**, або **Blind SQL Injection** за допомогою булевих умов, використовує логічні
вирази для ін'єкції, коли прямої відповіді від бази даних немає, але можна визначити стан (істина/хиба) за поведінкою
застосунку.

1. **Логічна умова в запиті**: атакуючий вставляє в запит булевий вираз, який завжди буде або істинним, або хибним. Це
   дозволяє перевіряти, як застосунок поводиться за різних умов.

2. **Аналіз поведінки відповіді**: якщо доданий вираз істинний, застосунок може повертати звичайну сторінку, а якщо
   хибний — інший тип відповіді (наприклад, помилка або порожня сторінка). Це дозволяє з’ясувати значення конкретних
   даних з бази.

   **Приклад POC**:
   Припустимо, запит `https://example.com/product?id=1` повертає сторінку з продуктом. Якщо атакуючий додає ін'єкцію:

https://example.com/product?id=1 AND 1=1

і сторінка завантажується нормально, значить, ін'єкція працює. Далі він змінює запит на:

https://example.com/product?id=1 AND 1=2 Якщо сторінка не завантажується або з'являється інша відповідь, це підтверджує
можливість використовувати Boolean Error Inferential Injection для витягування даних.

## Що таке Same-Origin Policy (SOP) і як він працює?

**Same-Origin Policy (SOP)** — це політика безпеки браузера, яка обмежує взаємодію між ресурсами, що завантажуються з
різних джерел. Ця політика захищає від несанкціонованого доступу до даних, дозволяючи взаємодію тільки між ресурсами, що
мають однакове джерело.

1. **Що означає "однакове джерело"**: два URL вважаються такими, що мають однакове джерело, якщо у них збігаються:
    - **Протокол** (наприклад, `http` або `https`),
    - **Домен** (наприклад, `example.com`),
    - **Порт** (наприклад, `:80` для HTTP або `:443` для HTTPS).

## Як працює варіант TE.TE у HTTP Request Smuggling

**TE.TE HTTP Request Smuggling** — це техніка, яка використовує різночитання в обробці заголовків `Transfer-Encoding`
і `Content-Length` між різними проксі-серверами або серверами для ін'єкції запиту. Цей варіант працює, коли сервери
обробляють заголовки `Transfer-Encoding: chunked` по-різному.

1. **Принцип роботи**: атака TE.TE експлуатує невідповідність в обробці заголовка `Transfer-Encoding: chunked` у першому
   сервері і невідповідність, коли другий сервер використовує `Content-Length` для визначення меж запиту.

2. **Процес атаки**:
    - Атакуючий надсилає запит з заголовком `Transfer-Encoding: chunked` до проксі-сервера.
    - Перший сервер обробляє цей заголовок і інтерпретує запит як "chunked", але передає його далі до іншого сервера,
      який ігнорує `Transfer-Encoding` і розглядає тіло запиту на основі `Content-Length`.
    - Це створює можливість розділити запит, ін’єктуючи додаткові HTTP-запити, які будуть оброблені другим сервером як
      окремі запити.

## Що таке DOM Clobbering і як його можна використати для обходу HTML-санацізаторів, що призводить до XSS?

**DOM Clobbering** — це техніка, що використовує властивості DOM для маніпуляції HTML-структурою, де змінюються або
перезаписуються певні елементи. Це може створити уразливості, особливо коли HTML-санацізатори пропускають деякі елементи
або атрибути, дозволяючи обхід і запуск шкідливого коду.

1. **Як це працює**: браузери автоматично прив’язують певні HTML-елементи до глобальних JavaScript-змінних на сторінці.
   Наприклад, елемент з атрибутом `name="form"` створює змінну `form`, доступну через `window.form`.

2. **Обхід HTML-санацізаторів**: оскільки деякі санацізатори можуть не перевіряти наявність атрибуту `name` або `id`, це
   дозволяє атакуючому створити елемент з маніпуляціями в DOM і перенаправити критичний функціонал (наприклад, замінити
   форму або скрипт).

3. **Приклад POC**:
   Якщо є код, що використовує `document.forms[0].submit()`, атакуючий може впровадити наступний код:
   ```html
   <meta name="form" content="javascript:alert(1)">
   <form id="form" name="form"></form>

## Як HTTP Parameter Pollution може обійти Web Application Firewall (WAF)

**HTTP Parameter Pollution (HPP)** — це техніка, яка використовує дублювання параметрів в одному HTTP-запиті, щоб обійти
правила валідації або захисту, застосовувані Web Application Firewall (WAF). WAF може обробляти або фільтрувати тільки
перший параметр, ігноруючи інші, що дозволяє обходити правила безпеки.

1. **Принцип роботи HPP**: атакуючий додає кілька однакових параметрів в URL або в тілі запиту, де WAF обробляє тільки
   перший параметр або не налаштований на виявлення такої конструкції.

2. **Метод обходу**:
    - Якщо WAF дозволяє лише певні значення параметрів, але не враховує дублювання, атакуючий може додати шкідливий
      параметр як другий.
    - Наприклад, якщо параметр `id` фільтрується за певним значенням, то запит `id=123&id=<script>alert(1)</script>`
      може обійти WAF, так як WAF дозволяє перший параметр, а додатковий ігнорує.

3. **Приклад POC**:
   Якщо є URL з параметром `id`, який захищений WAF:
   https://example.com/search?id=123

Атакуючий може дублювати параметр:
https://example.com/search?id=123&id=<script>alert(1)</script>

Якщо сервер обробляє другий параметр `id`, тоді як WAF дозволяє лише перший, це може призвести до виконання шкідливого
коду.

**HTTP Parameter Pollution** допомагає обійти WAF, експлуатуючи обробку дубльованих параметрів

## Що таке IDOR і чим його усунення відрізняється від інших уразливостей контролю доступу

**IDOR (Insecure Direct Object Reference)** — це уразливість, коли користувач може отримати доступ до об’єктів (файлів,
записів у базі даних тощо), змінюючи параметри, такі як `id`, без відповідної авторизації. Це дозволяє отримати доступ
до об’єктів інших користувачів або критичних даних, якщо система не перевіряє, чи належить об'єкт поточному
користувачеві.

1. **Принцип роботи**: користувач може змінити параметр `id` в URL або запиті, наприклад:

https://example.com/user/profile?id=100
замінивши значення `id` на інше, наприклад, `id=101`, для доступу до профілю іншого користувача.

2. **Відмінність від інших уразливостей контролю доступу**:

- **Тонка грань контролю доступу**: на відміну від загальних уразливостей контролю доступу, які часто реалізуються через
  ролі (наприклад, адміністратор проти звичайного користувача), IDOR зосереджується на доступі до конкретних об’єктів (
  даних) в межах однієї ролі.
- **Авторизація об'єкта**: для усунення IDOR необхідно перевіряти кожен запит, щоб підтвердити, що користувач має доступ
  до конкретного об’єкта, а не просто має загальний рівень доступу, як це реалізується для більшості уразливостей
  контролю доступу.

3. **Приклад POC**:
   Якщо на сайті `example.com` користувач має доступ до свого профілю за URL:
   і змінює параметр `id=100` на `id=101`, перевірка на стороні сервера повинна переконатися, що `id=101` дійсно
   належить поточному користувачеві, інакше це призведе до IDOR.

Для запобігання **IDOR** необхідно застосовувати точкову авторизацію для кожного об'єкта, що ускладнює реалізацію
порівняно з іншими механізмами контролю доступу, які працюють на рівні ролей або загальних прав.


