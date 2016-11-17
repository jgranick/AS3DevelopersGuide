## Restricting networking APIs {#restricting-networking-apis}

Flash Player 9 and later, Adobe AIR 1.0 and later

Networking APIs can be restricted in two ways. To prevent malicious activity, access to commonly reserved ports is blocked; you can’t override these blocks in your code. To control a SWF file’s access to network functionality with regard to other ports, you can use the allowNetworking setting.

**Blocked ports**

Flash Player 9 and later, Adobe AIR 1.0 and later

Flash Player and Adobe AIR have restrictions on HTTP access to certain ports, as do browsers. HTTP requests are not permitted to certain standard ports that are conventionally used for non-HTTP types of servers.

Any API that accesses a network URL is subject to these port blocking restrictions. The only exception is APIs that call sockets directly, such as Socket.connect() and XMLSocket.connect(), or calls to Security.loadPolicyFile() in which a socket policy file is being loaded. Socket connections are permitted or denied through the use of socket policy files on the target server.

The following list shows the ActionScript 3.0 APIs to which port blocking applies:

FileReference.download(),FileReference.upload(), Loader.load(), Loader.loadBytes(), navigateToURL(), NetConnection.call(), NetConnection.connect(), NetStream.play(), Security.loadPolicyFile(), sendToURL(), Sound.load(), URLLoader.load(), URLStream.load()

Port blocking also applies to Shared Library importing, the use of the &lt;img&gt; tag in text fields, and the loading of SWF files in an HTML page using the &lt;object&gt; and &lt;embed&gt; tags.

Port blocking also applies to the use of the &lt;img&gt; tag in text fields and the loading of SWF files in an HTML page using the &lt;object&gt; and &lt;embed&gt; tags.

The following lists show which ports are blocked: HTTP: 20 (ftp data), 21 (ftp control)

HTTP and FTP: 1 (tcpmux), 7 (echo), 9 (discard), 11 (systat), 13 (daytime), 15 (netstat), 17 (qotd), 19 (chargen),

22 (ssh), 23 (telnet), 25 (smtp), 37 (time), 42 (name), 43 (nicname), 53 (domain), 77 (priv-rjs), 79 (finger),

87 (ttylink), 95 (supdup), 101 (hostriame), 102 (iso-tsap), 103 (gppitnp), 104 (acr-nema), 109 (pop2), 110 (pop3),

111 (sunrpc), 113 (auth), 115 (sftp), 117 (uucp-path), 119 (nntp), 123 (ntp), 135 (loc-srv / epmap), 139 (netbios),

143 (imap2), 179 (bgp), 389 (ldap), 465 (smtp+ssl), 512 (print / exec), 513 (login), 514 (shell), 515 (printer),

526 (tempo), 530 (courier), 531 (chat), 532 (netnews), 540 (uucp), 556 (remotefs), 563 (nntp+ssl), 587 (smtp),

601 (syslog), 636 (ldap+ssl), 993 (ldap+ssl), 995 (pop3+ssl), 2049 (nfs), 4045 (lockd), 6000 (x11)

### Using the allowNetworking parameter {#using-the-allownetworking-parameter}

Flash Player 9 and later, Adobe AIR 1.0 and later

You can control a SWF file’s access to network functionality by setting the allowNetworking parameter in the

&lt;object&gt; and &lt;embed&gt; tags in the HTML page that contains the SWF content. Possible values of allowNetworking are:

• &quot;all&quot; (the default)—All networking APIs are permitted in the SWF file.

• &quot;internal&quot;—The SWF file may not call browser navigation or browser interaction APIs, listed later in this section, but it may call any other networking APIs.

• &quot;none&quot;—The SWF file may not call browser navigation or browser interaction APIs, listed later in this section, and it cannot use any SWF-to-SWF communication APIs, also listed later.

The allowNetworking parameter is designed to be used primarily when the SWF file and the enclosing HTML page are from different domains. Using the value of &quot;internal&quot; or &quot;none&quot; is not recommended when the SWF file being loaded is from the same domain as its enclosing HTML pages, because you can’t ensure that a SWF file is always loaded with the HTML page you intend. Untrusted parties could load a SWF file from your domain with no enclosing HTML, in which case the allowNetworking restriction will not work as you intended.

Calling a prevented API throws a SecurityError exception.

Add the allowNetworking parameter and set its value in the &lt;object&gt; and &lt;embed&gt; tags in the HTML page that contains a reference to the SWF file, as shown in the following example:

&lt;object classic=&quot;clsid:d27cdb6e-ae6d-11cf-96b8-444553540000&quot;

Code [base=&quot;http://fpdownload.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=9,0,124,](http://fpdownload.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version%3D9%2C0%2C124) 0&quot;

width=&quot;600&quot; height=&quot;400&quot; ID=&quot;test&quot; align=&quot;middle&quot;&gt;

**&lt;param name=&quot;allowNetworking&quot; value=&quot;none&quot; /&gt;**

&lt;param name=&quot;movie&quot; value=&quot;test.swf&quot; /&gt;

&lt;param name=&quot;bgcolor&quot; value=&quot;#333333&quot; /&gt;

**&lt;embed src=&quot;test.swf&quot; allowNetworking=&quot;none&quot; bgcolor=&quot;#333333&quot;**

width=&quot;600&quot; height=&quot;400&quot;

name=&quot;test&quot; align=&quot;middle&quot; type=&quot;application/x-shockwave-flash&quot; [pluginspage=&quot;http://www.macromedia.com/go/getflashplayer&quot;](http://www.macromedia.com/go/getflashplayer) /&gt;

&lt;/object&gt;

An HTML page may also use a script to generate SWF-embedding tags. You need to alter the script so that it inserts the proper allowNetworking settings. HTML pages generated by Adobe Flash Professional and Adobe Flash Builder use the AC_FL_RunContent() function to embed references to SWF files. Add the allowNetworking parameter settings to the script, as in the following:

AC_FL_RunContent( ... &quot;allowNetworking&quot;, &quot;none&quot;, ...)

The following APIs are prevented when allowNetworking is set to &quot;internal&quot;: navigateToURL(), fscommand(), ExternalInterface.call()

In addition to the APIs on the previous list, the following APIs are also prevented when allowNetworking is set to

&quot;none&quot;:

sendToURL(), FileReference.download(), FileReference.upload(), Loader.load(), LocalConnection.connect(), LocalConnection.send(), NetConnection.connect(), NetStream.play(), Security.loadPolicyFile(), SharedObject.getLocal(), SharedObject.getRemote(), Socket.connect(), Sound.load(), URLLoader.load(), URLStream.load(), XMLSocket.connect()

Even if the selected allowNetworking setting permits a SWF file to use a networking API, there may be other restrictions based on security sandbox limitations (see

“Security sandboxes” on page 1044

).

When allowNetworking is set to &quot;none&quot;, you cannot reference external media in an &lt;img&gt; tag in the htmlText

property of a TextField object (a SecurityError exception is thrown).

When allowNetworking is set to &quot;none&quot;, a symbol from an imported shared library added in the Flash Professional (not ActionScript) is blocked at run time.