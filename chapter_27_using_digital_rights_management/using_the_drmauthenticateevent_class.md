## Using the DRMAuthenticateEvent class {#using-the-drmauthenticateevent-class}

Adobe AIR 1.0 and later

The DRMAuthenticateEvent object is dispatched when a NetStream object tries to play protected content that requires a user credential for authentication before playback.

The DRMAuthenticateEvent handler is responsible for gathering the required credentials (user name, password, and type) and passing the values to the NetStream.setDRMAuthenticationCredentials() method for validation. Each AIR application must provide some mechanism for obtaining user credentials. For example, the application could provide a user with a simple user interface to enter the user name and password values. Also, provide a mechanism for handling and limiting repeated authentication attempts.

**DRMAuthenticateEvent properties**

Adobe AIR 1.0 and later

The DRMAuthenticateEvent class includes the following properties:

| **Property** | **Description** |
| --- | --- |
| authenticationType | Indicates whether the supplied credentials are for authenticating against Adobe Access (“drm”) or a proxy server (“proxy”). For example, the &quot;proxy&quot; option allows the application to authenticate against a proxy server if necessary before the user can access the Internet. Unless anonymous authentication is used, after the proxy authentication, the user must still authenticate against Adobe Access to obtain the voucher and play the content. You can use setDRMAuthenticationcredentials() a second time, with &quot;drm&quot; option, to authenticate against Adobe Access. |
| header | The encrypted content file header provided by the server. It contains information about the context of the encrypted content. |
| netstream | The NetStream object that initiated this event. |
| passwordPrompt | A prompt for a password credential, provided by the server. The string can include instruction for the type of password required. |
| urlPrompt | A prompt for a URL string, provided by the server. The string can provide the location where the user name and password are sent. |
| usernamePrompt | A prompt for a user name credential, provided by the server. The string can include instruction for the type of user name required. For example, a content provider can require an e-mail address as the user name. |

The previously mentioned strings are supplied by the FMRMS server only. Adobe Access Server does not use these strings.

### Creating a DRMAuthenticateEvent handler {#creating-a-drmauthenticateevent-handler}

Adobe AIR 1.0 and later

The following example creates an event handler that passes a set of hard-coded authentication credentials to the NetStream object that originated the event. (The code for playing the video and making sure that a successful connection to the video stream has been made is not included here.)

var connection:NetConnection = new NetConnection(); connection.connect(null);

var videoStream:NetStream = new NetStream(connection); videoStream.addEventListener(DRMAuthenticateEvent.DRM_AUTHENTICATE,

drmAuthenticateEventHandler)

private function drmAuthenticateEventHandler(event:DRMAuthenticateEvent):void

{

videoStream.setDRMAuthenticationCredentials(&quot;administrator&quot;, &quot;password&quot;, &quot;drm&quot;);

}

### Creating an interface for retrieving user credentials {#creating-an-interface-for-retrieving-user-credentials}

Adobe AIR 1.0 and later

In the case where protected content requires user authentication, the AIR application must usually retrieve the user’s authentication credentials via a user interface.

The following is a Flex example of a simple user interface for retrieving user credentials. It consists of a panel object containing two TextInput objects, one for each of the user name and password credentials. The panel also contains a button that launches the credentials() method.

&lt;mx:Panel x=&quot;236.5&quot; y=&quot;113&quot; width=&quot;325&quot; height=&quot;204&quot; layout=&quot;absolute&quot; title=&quot;Login&quot;&gt;

&lt;mx:TextInput x=&quot;110&quot; y=&quot;46&quot; id=&quot;uName&quot;/&gt;

&lt;mx:TextInput x=&quot;110&quot; y=&quot;76&quot; id=&quot;pWord&quot; displayAsPassword=&quot;true&quot;/&gt;

&lt;mx:Text x=&quot;35&quot; y=&quot;48&quot; text=&quot;Username:&quot;/&gt;

&lt;mx:Text x=&quot;35&quot; y=&quot;78&quot; text=&quot;Password:&quot;/&gt;

&lt;mx:Button x=&quot;120&quot; y=&quot;115&quot; label=&quot;Login&quot; click=&quot;credentials()&quot;/&gt;

&lt;/mx:Panel&gt;

The credentials() method is a user-defined method that passes the user name and password values to the setDRMAuthenticationCredentials() method. Once the values are passed, the credentials() method resets the values of the TextInput objects.

&lt;mx:Script&gt;

&lt;![CDATA[

public function credentials():void

{

}

]]&gt;

videoStream.setDRMAuthenticationCredentials(uName, pWord, &quot;drm&quot;); uName.text = &quot;&quot;;

pWord.text = &quot;&quot;;

&lt;/mx:Script&gt;

One way to implement this type of simple interface is to include the panel as part of a new state. The new state originates from the base state when the DRMAuthenticateEvent object is thrown. The following example contains a VideoDisplay object with a source attribute that points to a protected FLV file. In this case, the credentials() method is modified so that it also returns the application to the base state. This method does so after passing the user credentials and resetting the TextInput object values.

&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;

&lt;mx:WindowedApplication [xmlns:mx=&quot;http://www.adobe.com/2006/mxml&quot;](http://www.adobe.com/2006/mxml) layout=&quot;absolute&quot;

width=&quot;800&quot; height=&quot;500&quot; title=&quot;DRM FLV Player&quot;

creationComplete=&quot;initApp()&quot; &gt;

&lt;mx:states&gt;

&lt;mx:State name=&quot;LOGIN&quot;&gt;

&lt;mx:AddChild position=&quot;lastChild&quot;&gt;

&lt;mx:Panel x=&quot;236.5&quot; y=&quot;113&quot; width=&quot;325&quot; height=&quot;204&quot; layout=&quot;absolute&quot; title=&quot;Login&quot;&gt;

&lt;mx:TextInput x=&quot;110&quot; y=&quot;46&quot; id=&quot;uName&quot;/&gt;

&lt;mx:TextInput x=&quot;110&quot; y=&quot;76&quot; id=&quot;pWord&quot; displayAsPassword=&quot;true&quot;/&gt;

&lt;mx:Text x=&quot;35&quot; y=&quot;48&quot; text=&quot;Username:&quot;/&gt;

&lt;mx:Text x=&quot;35&quot; y=&quot;78&quot; text=&quot;Password:&quot;/&gt;

&lt;mx:Button x=&quot;120&quot; y=&quot;115&quot; label=&quot;Login&quot; click=&quot;credentials()&quot;/&gt;

&lt;mx:Button x=&quot;193&quot; y=&quot;115&quot; label=&quot;Reset&quot; click=&quot;uName.text=&#039;&#039;; pWord.text=&#039;&#039;;&quot;/&gt;

&lt;/mx:Panel&gt;

&lt;/mx:AddChild&gt;

&lt;/mx:State&gt;

&lt;/mx:states&gt;

&lt;mx:Script&gt;

&lt;![CDATA[

import flash.events.DRMAuthenticateEvent; private function initApp():void

{

videoStream.addEventListener(DRMAuthenticateEvent.DRM_AUTHENTICATE,

drmAuthenticateEventHandler);

}

public function credentials():void

{

videoStream.setDRMAuthenticationCredentials(uName, pWord, &quot;drm&quot;); uName.text = &quot;&quot;;

pWord.text = &quot;&quot;; currentState=&#039;&#039;;

}

private function drmAuthenticateEventHandler(event:DRMAuthenticateEvent):void

{

currentState=&#039;LOGIN&#039;;

}

]]&gt;

&lt;/mx:Script&gt;

&lt;mx:VideoDisplay id=&quot;video&quot; x=&quot;50&quot; y=&quot;25&quot; width=&quot;700&quot; height=&quot;350&quot; autoPlay=&quot;true&quot;

bufferTime=&quot;10.0&quot; [source=&quot;http://www.example.com/flv/Video.flv&quot;](http://www.example.com/flv/Video.flv) /&gt;

&lt;/mx:WindowedApplication&gt;