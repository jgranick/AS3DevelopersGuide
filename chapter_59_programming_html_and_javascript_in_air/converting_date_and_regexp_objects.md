## Converting Date and RegExp objects {#converting-date-and-regexp-objects}

Adobe AIR 1.0 and later

The JavaScript and ActionScript languages both define Date and RegExp classes, but objects of these types are not automatically converted between the two execution contexts. You must convert Date and RegExp objects to the equivalent type before using them to set properties or function parameters in the alternate execution context.

For example, the following ActionScript code converts a JavaScript Date object named jsDate to an ActionScript Date object:

var asDate:Date = new Date(jsDate.getMilliseconds());

The following ActionScript code converts a JavaScript RegExp object named jsRegExp to an ActionScript RegExp object:

var flags:String = &quot;&quot;;

if (jsRegExp.dotAll) flags += &quot;s&quot;; if (jsRegExp.extended) flags += &quot;x&quot;; if (jsRegExp.global) flags += &quot;g&quot;;

if (jsRegExp.ignoreCase) flags += &quot;i&quot;; if (jsRegExp.multiline) flags += &quot;m&quot;;

var asRegExp:RegExp = new RegExp(jsRegExp.source, flags);