# Differences between Anchors, Inputs, and Buttons

- CSS can make one element lok like another and behave the same also.
- How do you know which one to use, then?

Anchor <a>
----

- represent hyperlinks

Input `<input/>`
----

- represents a data field

|Attribute|Example|Meaning|
|--|--|--|
|value|`<input type="text" id="lname" name="lname" value="Smith">` | specifies an initial value for the input field|
|readonly|`<input ... readonly>`|specifies input field is read-only|
|disabled|`<input ... disabled>`|specifies input field is disabled|
|size|`<input ... size="50">`|specifies input field visible length|
|maxlength|`<input ... maxlength="4">`|specifies max # of character allowed|
|min/max|`<input ... min="1" max="5">`|specifies min/max values|
|multiple|`<input ... multiple">`|allows to enter more than one value|
|pattern|`<input ... pattern="[A-Za-z]{3}">`|a regular expression which value is checked against|
|placeholder|`<input ... placeholder="123">`|specifies short hint describes expected value of the field|
|required|`<input ... required">`|specifies input field is required|
|step|`<input ... step="3">`|specifies the legal number of intervals|
|autofocus|`<input ... autofocus>`|specifies the field gets focus when page is loaded|
|hight/width|`<input ... width="48" height="48">`|specifies width/height |
|autocomplete|`<input ... autocomplete="on">`|specifies autocomplete on or off|

- "list" attribute

```html
    <form>
        <input list="browsers">
        <datalist id="browsers">
            <option value="Internet Explorer">
            <option value="Firefox">
            <option value="Chrome">
            <option value="Opera">
            <option value="Safari">
        </datalist>
    </form>
```

- "type" attribute values

```html
    <input type="button">
    <input type="checkbox">
    <input type="color">
    <input type="date">
    <input type="datetime-local">
    <input type="email">
    <input type="file">
    <input type="hidden">
    <input type="image">
    <input type="month">
    <input type="number">
    <input type="password">
    <input type="radio">
    <input type="range">
    <input type="reset">
    <input type="search">
    <input type="submit">
    <input type="tel">
    <input type="text">
    <input type="time">
    <input type="url">
    <input type="week">
```

Button `<button>`
----

- represents a button!

- Buttons do the same things as the inputs. 

- Buttons were introduced into HTML as an alternative to inputs that are much easier to style.

- Unlike inputs, a button's label is determined by its content. This means you can nest elements within a button, such as images, paragraphs, or headers.

- Buttons can also contain ::before and ::after pseudo-elements.

- Attributes

|Attribute|    Value    |Description|
|--|--|--|
|autofocus|    autofocus|    Specifies that a button should automatically get focus when the page loads|
|disabled|    disabled|    Specifies that a button should be disabled|
|form|    form_id|    Specifies which form the button belongs to|
|formaction|    URL|    Specifies where to send the form-data when a form is submitted. Only for type="submit"|
|formenctype|    application/x-www-form-urlencoded multipart/form-data text/plain|Specifies how form-data should be encoded before sending it to a server. Only for type="submit"|
|formmethod|    get post|    Specifies how to send the form-data (which HTTP method to use). Only for type="submit"|
|formnovalidate|    formnovalidate|    Specifies that the form-data should not be validated on submission. Only for type="submit"|
|formtarget    _blank _self _parent _top framename|    Specifies where to display the response after submitting the form. Only for type="submit"|
|name|    name    |Specifies a name for the button|
|type|    button reset| submit    Specifies the type of button|
|value|    text|    Specifies an initial value for the button|
|disabled|disabled|Specifies the button is disabled|




