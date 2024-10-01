# JS-Widget
This repository contains sample JS-Widgets applications for [iOS WidgetWeb App](https://apps.apple.com/app/widget-web/id1522169352)  
You can use them to make own JS-Widgets.

The main idea of ​​JS-Widgets is to allow developers to create iOS widgets using the web development stack (HTML+CSS+JS).  
The JS-Widgets feature will always be free for all.

# Publishing your own JS-Widgets

If you have developed your own JS-Widget and want to share it with others, you can distribute the URL to people and they will use it in the [iOS WidgetWeb App](https://apps.apple.com/app/widget-web/id1522169352).  
Or you can write to [me](mailto:supporrt.vitalek.app@gmail.com) to add this URL to the [iOS WidgetWeb App](https://apps.apple.com/app/widget-web/id1522169352) JS-Widgets list.

# Debugging your own JS-Widgets

In the [WidgetWeb](https://apps.apple.com/app/widget-web/id1522169352) App menu, you can enable Developer mode.  
In this mode you can view the logs of your JS-Widget.  
Use `__wweb2Log("message")` to send to log from js code.

```js
---------------------------------------------------------------------------
19:24:44.280| @@App::applicationDidFinishLaunching: Version:7.1 (5758)
19:24:44.506| @@App::applicationDidBecomeActive
19:33:15.764| @@Start Load URL=https://vitalek.app/jsWidget/CounterJsWidget/index.html
19:33:15.907| @@++WebPage load progress:10.0 %
19:33:16.167| @@++WebPage load progress:50.0 %
19:33:16.179| @@++WebPage load progress:100.0 %
19:33:16.194| __wweb2WaitMillisecondsToWidgetIsReady(currentDelayMs=1000) found -> return new delay, msec: 0
19:33:16.300| __wweb2WaitMillisecondsToWidgetIsReady(currentDelayMs=0) found -> return new delay, msec: 0
19:33:16.405| @@++WebPage is ready
19:33:21.410| @@WEB2-LOG: findAllWidgetButtons. buttonIndex:0 rect:[x=26.65625 y=26.65625 width=336.53125 height=336.53125] style={"selfColor":"rgb(0, 0, 0)","wwebBtnColor":"","wwebBtnPressColor":"rgba(150, 0, 0, 0.7)","wwebBtnCornerRadius":"18","wwebBtnPadding":"-1","wwebBtnDisabled":""} tagName=A fastJsButtonURLStr= https://vitalek.app/jsWidget/CounterJsWidget/index.html#action=minus
19:33:21.410| @@WEB2-LOG: findAllWidgetButtons. buttonIndex:1 rect:[x=461.2101135253906 y=239.078125 width=57.579803466796875 height=83] style={"selfColor":"rgb(0, 0, 0)","wwebBtnColor":"rgba(0, 0, 0, 0)","wwebBtnPressColor":"rgba(150, 0, 0, 0.3)","wwebBtnCornerRadius":"50","wwebBtnPadding":"-25 -50% -15 -50%","wwebBtnDisabled":""} tagName=A fastJsButtonURLStr= https://vitalek.app/jsWidget/CounterJsWidget/index.html#action=reset
19:33:21.410| @@WEB2-LOG: findAllWidgetButtons. buttonIndex:2 rect:[x=616.8125 y=26.65625 width=336.53125 height=336.53125] style={"selfColor":"rgb(0, 0, 0)","wwebBtnColor":"","wwebBtnPressColor":"rgba(0, 150, 0, 0.7)","wwebBtnCornerRadius":"18","wwebBtnPadding":"-1","wwebBtnDisabled":""} tagName=A fastJsButtonURLStr= https://vitalek.app/jsWidget/CounterJsWidget/index.html#action=plus
```

# How JS-Widgets work

The JS-Widget web page runs in a WKWebView (Safari) inside an iOS widget.  
Once the page is loaded and ready, the [WidgetWeb](https://apps.apple.com/app/widget-web/id1522169352) takes a screenshot of the page and displays it to the user.  
WidgetWeb also tries to find all [< a >](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a) and [< button >](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button) inside the final JS-Widget web page and create a native SwiftUI buttons in iOS widget so that the user can click on them.  
It is better to use ***< a >*** as they work faster in JS-Widgets.

# Important
iOS Homescreen widgets apps are very limited in memory (30MB) and CPU.  
So, do not load large images in your JS-Widget or widget will fail with out of memory.

# API

Check out sample JS-Widgets.

## JS-Widget has only one mandatory requirement.
JS-Widget **MUST** return a **title**. There are 2 options for returning a title:
 
 - Define `<meta name="js-widget-title" content="Tic-Tac-Toe">` in `<head>` section of widget main html file.
 ```html 
<html>
  <head>
     <meta  name="js-widget-title" content="Tic-Tac-Toe">
  .....
 ```
 - Or define global `function __wweb2JsWidgetTitle()`
 ```js
function __wweb2JsWidgetTitle() {
	return "Tic-Tac-Toe";
}
```

## JS-Widget State and Cache Management

iOS widgets are very limited in RAM. Only 30 MB is available for all widgets in the app. 
So the only way to solve the problem is to minimize memory usage by design.  
To achieve this, the widget does not store the active browser instance in memory, but loads it when necessary.  
To speed up page loading, the widget caches the main page of the JS-Widget by default and only the widget's Reload (↻) button will make a request to the origin page. 
In other cases, the widget uses the cached main page file.

It is the developer's responsibility to control the caching of other files loaded from the JS-Widget main page. It is better to cache all files to speed up the response of JS-Widget.

JS-Widget SHOUD store its state in cookies. Cookies are shared between all JS-Widget instances and the main application.

On any reload, the JS-Widget MUST restore the same appearance as before reload.


## How to make JS-Widget interactive buttons

The widget tries to find all `<a>` and `<button>` elements on the page to allow user interaction with them from iOS widget. But these elements have a very big difference in the workflow.

Please note that the widget does not have a running browser with the JS-Widget page, but always loads it after user interaction due to memory limitations.

It is better to use `<a>` since the widget will collect the `href` URL and use it to load the page when the user clicks on `<a>`.  
On the contrary, `<button>` does not have its own URL, but uses some js code. To do this, the widget will load the source page into memory, find the corresponding `<button>` element, and programmatically click it. So, running `<button>` will waste a lot of time loading the entire page just to click on the `<button>` element.

In `<a>` elements, it is better to use the hash (#) part of the URL to send interaction commands. Because the hash part (#) of the URL is ignored by the cache and allows for faster interaction of the JS-Widget.

#### Interactive buttons example:
```js
<a  class="plus-btn"  id="plus_btn"  href="#action=plus"  onclick="event.preventDefault(); performPlusBtn();">+</a>
```
This `<a>` element uses `href="#action=plus"` to interact within the JS-Widget and `onclick="event.preventDefault(); performPlusBtn();"` for normal browser interaction.

When a user taps an iOS widget, the widget's internal browser will load the page defined in the `<a href>`.  
And the JS code will get the hash (#) part of the URL and execute it's command.

```js
<script type='text/javascript'>
    function performPlusBtn() {
        counterValue += 1;
        setCookie("counterValue", counterValue+"", 365000);
        updateCounterValue();
    }


    // process buttons press in jswidget
    let queryString = window.location.hash;
    if (queryString.startsWith("#")) {
        queryString = queryString.substring(1)
    }
    const urlParams = new URLSearchParams(queryString);
    const actionParam = urlParams.get('action');
    if (actionParam) {
        switch (actionParam) {
            case "plus": {
                performPlusBtn();
            };break;
            case "minus": {
                performMinusBtn();
            };break;
            case "reset": {
                performResetBtn();
            };break;
        }
    }
</script>
```

## Loading Asynchronous Data in JS-Widget

To give JS-Widget extra time to load page content, there is a special `function __wweb2WaitMillisecondsToWidgetIsReady(currentDelayMs)`.  
The iOS widget will call this function multiple times to check if the JS-Widget is ready to show it's state to the user.  
In this function you get the current delay in milliseconds `(currentDelayMs)` that the widget wants to use, and you can return another delay to get additional time to load. Maximum 30 seconds.  
By default, the widget will wait 1 second after the page has loaded. To achieve maximum interaction speed use `return 0;`

```js
function  __wweb2WaitMillisecondsToWidgetIsReady(currentDelayMs) {
    return  0;
}
```


## CSS Extensions

There are special CSS styles that control the appearance of buttons in the iOS widget.
```css
--wweb2-overlay-btn-disabled: true;
--wweb2-overlay-btn-color: rgba(0, 0, 0, 0.0);
--wweb2-overlay-btn-press-color: rgba(150, 0, 0, 0.3);
--wweb2-overlay-btn-corner-radius: 50%;
--wweb2-overlay-btn-padding: -25%  -50%  -15%  -50%;
```

 - `--wweb2-overlay-btn-disabled: true;` - Set `true` to disable user interaction for `<a> or <button>` elements.
 - `--wweb2-overlay-btn-color: rgba(0, 0, 0, 0.0);` - The color of the overlay button displayed in the iOS widget over of the web page.
 - `--wweb2-overlay-btn-press-color: rgba(150, 0, 0, 0.3);` - The color of the overlay button that appears in the iOS widget over the web page when the user click it.
 - `--wweb2-overlay-btn-corner-radius: 50%;` - Corner radius for the overlay button displayed in the iOS widget, over the web page.
 - `--wweb2-overlay-btn-padding: -25%  -50%  -15%  -50%;` - Padding for the overlay button displayed in the iOS widget, over the web page. You can set a negative value to make the button larger.
