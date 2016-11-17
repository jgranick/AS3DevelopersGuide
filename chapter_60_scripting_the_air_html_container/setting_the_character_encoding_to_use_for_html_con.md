## Setting the character encoding to use for HTML content {#setting-the-character-encoding-to-use-for-html-content}

Adobe AIR 1.0 and later

An HTML page can specify the character encoding it uses by including meta tag, such as the following:

meta http-equiv=&quot;content-type&quot; content=&quot;text/html&quot; charset=&quot;ISO-8859-1&quot;;

Override the page setting to ensure that a specific character encoding is used by setting the textEncodingOverride

property of the HTMLLoader object:

var html:HTMLLoader = new HTMLLoader(); html.textEncodingOverride = &quot;ISO-8859-1&quot;;

Specify the character encoding for the HTMLLoader content to use when an HTML page does not specify a setting with the textEncodingFallback property of the HTMLLoader object:

var html:HTMLLoader = new HTMLLoader(); html.textEncodingFallback = &quot;ISO-8859-1&quot;;

The textEncodingOverride property overrides the setting in the HTML page. And the textEncodingOverride

property and the setting in the HTML page override the textEncodingFallback property.

Set the textEncodingOverride property or the textEncodingFallback property before loading the HTML content.