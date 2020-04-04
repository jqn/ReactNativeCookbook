# gotchas

## Navigator

Prevent memory leaks use navigation experimental instead of navigator

Think right about where to handle logic  
!fetch in vinscanner component  
handle in the next screen

## WebView

* OnMessage: if trying to trigger alerts from within the WebView import Alert to your component. Alerts wont' work otherwise.
* postMessage: this won't work on click unless calling preventDefault\(\) or wrap it with `$(document).ready(function() {});`
* Setting onMessage on a WebView overrides existing values of window.postMessage,  but a previous value was defined.
* Weird white flash on mount - make sure the WebView has a preferred background color.

