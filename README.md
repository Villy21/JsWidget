# JS-Widget
This repository contains sample JS-Widgets applications for [iOS WidgetWeb App](https://apps.apple.com/app/widget-web/id1522169352)  
You can use them to make own JS-Widgets.

The main idea of ​​JS-Widgets is to allow developers to create iOS widgets using the web development stack (HTML+CSS+JS).  
The JS-Widgets feature will always be free for all.

# Publishing your own JS-Widgets

If you have developed your own JS-Widget and want to share it with others, you can distribute the URL to people and they will use it in the [iOS WidgetWeb App](https://apps.apple.com/app/widget-web/id1522169352).  
Or you can write to [me](mailto:supporrt.vitalek.app@gmail.com) to add this URL to the [iOS WidgetWeb App](https://apps.apple.com/app/widget-web/id1522169352) JS-Widgets list.

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
