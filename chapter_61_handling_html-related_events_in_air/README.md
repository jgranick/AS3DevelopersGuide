# Chapter 61: Handling HTML-related events in AIR {#chapter-61-handling-html-related-events-in-air}

Adobe AIR 1.0 and later

An event-handling system allows programmers to respond to user input and system events in a convenient way. The Adobe® AIR® event model is not only convenient, but also standards-compliant. Based on the Document Object Model (DOM) Level 3 Events Specification, an industry-standard event-handling architecture, the event model provides a powerful, yet intuitive, event-handling tool for programmers.

**HTMLLoader events**

Adobe AIR 1.0 and later

An HTMLLoader object dispatches the following Adobe® ActionScript® 3.0 events:

| **Event** | **Description** |
| --- | --- |
| htmlDOMInitialize | Dispatched when the HTML document is created, but before any scripts are parsed or DOM nodes are added to the page. |
| complete | Dispatched when the HTML DOM has been created in response to a load operation, immediately after the onload event in the HTML page. |
| htmlBoundsChanged | Dispatched when one or both of the contentWidth and contentHeight |
| locationChange | Dispatched when the location property of the HTMLLoader has changed. |
| locationChanging | Dispatched before the location of the HTMLLoader changes because of user navigation, a JavaScript call, or a redirect. The locationChanging event is not dispatched when you call the load(), loadString(), reload(), historyGo(), historyForward(), or historyBack() methods. |
| scroll | Dispatched anytime the HTML engine changes the scroll position. Scroll events can be because of navigation to anchor links (# links) in the page or because of calls to the window.scrollTo() method. Entering text in a text input or text area can also cause a scroll event. |
| uncaughtScriptException | Dispatched when a JavaScript exception occurs in the HTMLLoader and the exception is not caught in JavaScript code. |

You can also register an ActionScript function for a JavaScript event (such as onClick). For details, see

“Handling

DOM events with ActionScript” on page 1021

.