# materialize-autocomplete
Materialize-css styled autocomplete, examples:  [https://icefox0801.github.io/materialize-autocomplete/examples/](https://icefox0801.github.io/materialize-autocomplete/examples/)

## A hard fork for a reason
This is a hard form from [ValYouw's](https://github.com/ValYouW/materialize-autocomplete)'s fork,  which is a fork from [Icefox  Materialize Autocomplete](https://github.com/icefox0801/materialize-autocomplete). It seems to work ok most of the time. Perhaps we can make it even better.

There is a good reason for why this is a hard fork. At the time of writing Meteor Atmosphere Packages can only include published NPM packages. The styling of the pills cannot be configured to not show the ids, and the existing forks seem stale... While MaterializeCSS is implementing a built in autocomplete with support for multiple, there is no choice but a hard fork.

This fork is meant for use with Meteor Autoform Materialize. It is not meant as a replacement for the existing repos. It exists mainly due to dependency chains.

We thank all thee that has gone before as and wish those behind us good luck.
Going forward this fork will be managed by [ExpertBox.com](https://www.ExpertBox.com/home) as a big thank you to the Open Source community.

## Install
+ npm
```shell
$ npm install materialize-autocomplete2
```

## Usage
<!-- ![autocomplete](https://cloud.githubusercontent.com/assets/3138397/17131670/1cca05be-5351-11e6-8c77-1d9a98ab765c.gif) -->
+ When typing inside an input, autocomplete prompts will show on dropdown list
+ Choosing an option by click or `↑`, `↓`, `Enter` keys
+ Removing a selection by click `x` when enable multiple selection

```javascript
var autocomplete = $('#el').materialize_autocomplete({
    limit: 20,
    multiple: {
        enable: true,
        maxSize: 10,
        onExist: function (item) { /* ... */ },
        onExceed: function (maxSize, item) { /* ... */ }
    },
    appender: {
        el: '#someEl'
    },
    getData: function (value, callback) {
        // ...
        callback(value, data);
    }
});
```

## $.fn.materialize_autocomplete
+ `$.fn.materialize_autocomplete(options) [function]`: Initialize an `autocomplete` widget and return an `Autocomplete` instance

## Autocomplete options
+ `cacheable [boolean]`: Dropdown data cacheable or not, default: `true`
+ `limit [number]`: Max number of items show in dropdown, default: `10`
+ `multiple [object]`: Configuration of multiple selection, seeing [properties of multiple](#properties-of-multiple) for more details
+ `hidden [object]`: Configuration of hidden input (used for storing **ids** joined by selections or **id** of a selection), seeing [properties of hidden](#properties-of-hidden) for more details
+ `appender [object]`: Configuration of appender (where to append selections when multiple selection is enabled), seeing [properties of appender](#properties-of-appender) for more details
+ `dropdown [object]`: Configuration of dropdown, seeing [properties of dropdown](#properties-of-dropdown) for more details
+ `onSelect(item) [function]`: Callback function after selecting an item in single selection mode
+ `getData(value, callback) [function]`: Function for getting dropdown list data, asynchronous called with a `callback`
    + `value [string]`: Input value，when `input` event triggered, `getData` will be called with input value
    + `callback(value, data) [function]`: Callback function
        + `value [string]`: Same as `value` above
        + `data [array]`: Data array，should be formatted as `[{ 'id': '1', 'text': 'a' }, { 'id': '2', 'text': 'b'}]`
+ `ignoreCase [boolean]`: Ignore case or not, default: `true`
+ `throttling [boolean]`: Throttling for `getData` function or not, default: `true`


## Autocomplete
### Constructor
+ `new Autocomplete(el, options) [function]`: Constructor
    + `el [string|object]`: DOM element on which the widget applys
    + `options [object]`: Configuration of the widget

### Instance property:
+ `autocomplete.options [object]`: Configuration object
+ `autocomplete.$el [object]`: JQuery object on which the widget applys
+ `autocomplete.$wrapper [object]`: Wrapper jQuery object，equal to `autocomplete.$el.parent()`
+ `autocomplete.compiled [object]`: Compiling functions for `tagTemplate` and `itemTemplate`
+ `autocomplete.$dropdown [object]`: JQuery object of dropdown
+ `autocomplete.$appender [object]`: JQuery object of appender
+ `autocomplete.$hidden [object]`: JQuery object of hidden input
+ `autocomplete.resultCache [object]`: Result cache object of `getData` when `cacheable` is `true`
+ `autocomplete.value [array]`: Value of widget, when multiple selection is enabled, `autocomplete.value` is **ids** joined by selections, otherswise `autocomplete.value` is **id** of a selection

### Instance methods:
+ `autocomplete.initialize() [function]`: Initializing method
+ `autocomplete.render() [function]`: Rendering method
+ `autocomplete.setValue(item) [function]`: Value setting method
    + `item [object]`: Selection object, e.g. `{ id: '1', text: 'a'}`
+ `autocomplete.append(item) [function]`: Appending an selection, called when `options.multiple.enable` is `true`
+ `autocomplete.remove(item) [function]`: Removing an selection, called when `options.multiple.enable` is `true`
+ `autocomplete.select(item) [function]`: Setting the value, called when `options.multiple.enable` is `false`

## Detailed options
### Properties of multiple
|property|description|default|
|:---|:---|:---|
|`enable [boolean]`|Enabled or not|`false`|
|`maxSize [number]`|Max number of selections|`4`|
|`onExist [function]`|Callback when selection to append exists||
|`onExceed [function]`|Callback when selections exceed `maxSize`||
|`onAppend [function]`|Callback after appending a selection||
|`onRemove [function]`|Callback after removing a selection||
### Properties of hidden
|property|description|default|
|:---|:---|:---|
|`enable [boolean]`|Enabled or not|`false`|
|`el [string|object]`|Applying an existing DOM element if not null, otherwise created one|`''`|
|`inputName [string]`|`name` attribute of hidden input|`''`|
|`required [boolean]`|`required` attribute of hidden input|`false`|
### Properties of appender
|property|description|default|
|:---|:---|:---|
|`el [string|object]`|Applying an existing DOM element if not null, otherwise created one|`''`|
|`tagName [string]`|`TagName` of appender when `appender.el` is null|`ul`|
|`className [string]`|`ClassName` attribute of appender|`ac-appender`|
|`tagTemplate [string]`|Template string of selections inside appender||
Note that `tagTemplate` should use [undescore template](http://underscorejs.org/#template) semantic, **`data-id` and `data-text` attributes should be specified on outest DOM**
### Properties of dropdown
|property|description|default|
|:---|:---|:---|
|`el [string|object]`|Applying an existing DOM element if not null, otherwise created one|`''`|
|`tagName [string]`|`TagName` of dropdown when `dropdown.el` is null|`ul`|
|`className [string]`|`ClassName` attribute of dropdown|`ac-dropdown`|
|`itemTemplate [string]`|Template string of items inside dropdown||
|`noItem [string]`|Prompt for no data|`''`|
Note that `itemTemplate` should use [undescore template](http://underscorejs.org/#template), **`data-id` and `data-text` attributes should be specified on outest DOM**
