# Chapter 27: Using digital rights management {#chapter-27-using-digital-rights-management}

Flash Player 10.1 and later, Adobe AIR 1.5 and later

Adobe® Access™ is a content protection solution. It lets content owners, distributors, and advertisers realize new sources of revenue by providing seamless access to premium content. Publishers use Adobe Access to encrypt content, create policies, and issue licenses. Adobe Flash Player and Adobe AIR incorporate a DRM library, the Adobe Access module. This module enables the runtime to communicate with the Adobe Access license server and play back protected content. The runtime thus completes the life cycle of content protected by Adobe Access and distributed by Flash Media Server.

With Adobe Access, content providers can provide both free content and premium content. For example, a consumer wants to view a television program without advertisements. The consumer registers and pays the content publisher. The consumer can then enter their user credentials to gain access and play the program without the ads.

In another example, a consumer wants to view content offline while traveling with no Internet access. This offline workflow is supported in AIR applications. After registering and paying the publisher for the content, the user can access and download the content and associated AIR application from the publisher’s website. Using the AIR application, the user can view the content offline during the permitted period. The content can also be shared with other devices in the same device group using domains (**New in 3.0**).

Adobe Access also supports anonymous access, which does not require user authentication. For example, a publisher can use anonymous access to distribute ad-supported content. Anonymous access also lets a publisher allow free access to the current content for a specified number of days. The content provider can also specify and restrict the type and version of the player needed for their content.

When a user tries to play protected content in Flash Player or Adobe AIR, your application must call the DRM APIs. The DRM APIs initiate the workflow for playback of protected content. The runtime, through the Adobe Access module, contacts the license server. The license server authenticates the user, if necessary, and issues a license to allow playback of protected content. The runtime receives the license and decrypts the content for playback.

How to enable your application to play content protected by Adobe Access is described here. It is not necessary to understand how to encrypt content or maintain policies using Adobe Access. However, it is assumed that you are communicating with the Adobe Access license server to authenticate the user and retrieve the license. It is also assumed that you are designing an application to specifically play content protected by Adobe Access.

For an overview of Adobe Access, including creating policies, see the documentation included with Adobe Access.

**More Help topics**

[flash.net.drm package](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/net/drm/package-detail.html) [flash.net.NetConnection](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/net/NetConnection.html) [flash.net.NetStream](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/net/NetStream.html)

**Understanding the protected content workflow**

Flash Player 10.1 and later, Adobe AIR 2.0 and later

**_Important_**: Flash Player 11.5 and above integrates the Adobe Access module, so the update step (calling SystemUpdater.update(SystemUpdaterType.DRM)) is unnecessary. This includes the following browsers and platforms:

• Flash Player 11.5 ActiveX control, for all platforms except Internet Explorer on Windows 8 on Intel processors

• Flash Player 11.5 plugin, for all browsers

• Adobe AIR (desktop and mobile)

This means that the update step is _still required_ in the following cases:

• Internet Explorer on Windows 8 on Intel processors

• Flash Player 11.4 and below, except on Google Chrome 22 and above (all platforms) or 21 and above (Windows)

**_Note:_ **_You can still safely call SystemUpdater.update(SystemUpdaterType.DRM)on a system with Flash Player 11.5 or higher, but nothing is downloaded._

The following high-level workflow shows that how an application can retrieve and play protected content. The workflow assumes that the application is designed specifically to play content protected by Adobe Access:

1.  Get the content metadata.
2.  Handle updates to Flash Player, if needed.
3.  Check if a license is available locally. If so, load it and go to step 7\. If not, go to step 4.
4.  Check if authentication is required. If not, you can go to step 7.
5.  If authentication is required, get the authentication credentials from the user and pass them to the license server.
6.  If domain registration is required, join the domain (AIR 3.0 and higher).
7.  Once authentication succeeds, download the license from the server.
8.  Play the content.

If an error has not occurred and the user was successfully authorized to view the content, the NetStream object dispatches a DRMStatusEvent object. The application then begins playback. The DRMStatusEvent object holds the related voucher information, which identifies the user’s policy and permissions. For example, it holds information regarding whether the content can be made available offline or when the license expires. The application can use this data to inform the user of the status of their policy. For example, the application can display the number of remaining days the user has for viewing the content in a status bar.

If the user is allowed offline access, the voucher is cached, and the encrypted content is downloaded to the user’s machine. The content is made accessible for the duration defined in the license caching duration. The detail property in the event contains &quot;DRM.voucherObtained&quot;. The application decides where to store the content locally in order for it to be available offline. You can also preload vouchers using the DRMManager class.

**_Note:_ **_Caching and pre-loading of vouchers is supported in both AIR and Flash Player. However, downloading and storing encrypted content is supported only in AIR._

It is the application’s responsibility to explicitly handle the error events. These events include cases where the user inputs valid credentials, but the voucher protecting the encrypted content restricts the access to the content. For example, an authenticated user cannot access content if the rights have not been paid for. This case can also occur when two registered members of the same publisher attempt to share content that only one of them has paid for. The application must inform the user of the error and provide an alternative suggestion. A typical alternative suggestion is instructions in how to register and pay for viewing rights.

**Detailed API workflow**

Flash Player 10.1 and later, AIR 2.0 and later

This workflow provides a more detailed view of the protected-content workflow. This workflow describes the specific APIs used to play content protected by Adobe Access.

1.  Using a URLLoader object, load the bytes of the protected content’s metadata file. Set this object to a variable, such as metadata_bytes.

All content controlled by Adobe Access has Adobe Access metadata. When the content is packaged, this metadata can be saved as a separate metadata file (.metadata) alongside the content. For more information, see the Adobe Access documentation.

1.  Create a DRMContentData instance. Put this code into a try-catch block:

new DRMContentData(_metadata_bytes_)

where _metadata_bytes_ is the URLLoader object obtained in step 1.

1.  (Flash Player only) The runtime checks for the Adobe Access module. If not found, an IllegalOperationError with DRMErrorEvent error code 3344 or DRMErrorEvent error code 3343 is thrown.

To handle this error, download the Adobe Access module using the SystemUpdater API. After this module is downloaded, the SystemUpdater object dispatches a COMPLETE event. Include an event listener for this event that returns to step 2 when this event is dispatched. The following code demonstrates these steps:

flash.system.SystemUpdater.addEventListener(Event.COMPLETE, updateCompleteHandler); flash.system.SystemUpdater.update(flash.system.SystemUpdaterType.DRM)

private function updateCompleteHandler (event:Event):void {

/*redo step 2*/

drmContentData = new DRMContentData(metadata_bytes);

}

If the player itself must be updated, a status event is dispatched. For more information on handling this event, see

“Listening for an update event” on page 545

.

**_Note:_ **_In AIR applications, the AIR installer handles updating the Adobe Access module and required runtime updates._

1.  Create listeners to listen for the DRMStatusEvent and DRMErrorEvent dispatched from the DRMManager object:

DRMManager.addEventListener(DRMStatusEvent.DRM_STATUS, onDRMStatus); DRMManager.addEventListener(DRMErrorEvent.DRM_ERROR, onDRMError);

In the DRMStatusEvent listener, check that the voucher is valid (not null). In the DRMErrorEvent listener, handle DRMErrorEvents. See

“Using the DRMStatusEvent class” on page 537

and

“Using the DRMErrorEvent class” on

page 541

.

1.  Load the voucher (license) that is required to play the content. First, try to load a locally stored license to play the content:

DRMManager.loadvoucher(_drmContentData_, LoadVoucherSetting.LOCAL_ONLY)

After loading completes, the DRMManager object dispatches DRMStatusEvent.DRM_Status.

1.  If the DRMVoucher object is not null, the voucher is valid. Skip to step 13.
2.  If the DRMVoucher object is null, check the authentication method required by the policy for this content. Use the

DRMContentData.authenticationMethod property.

1.  If the authentication method is ANONYMOUS, go to step 13.
2.  If the authentication method is USERNAME_AND_PASSWORD, your application must provide a mechanism to let the user enter credentials. Pass these credentials to the license server to authenticate the user:

DRMManager.authenticate(_metadata_.serverURL, _metadata_.domain, _username_, _password_)

The DRMManager dispatches a DRMAuthenticationErrorEvent if authentication fails or a

DRMAuthenticationCompleteEvent if authentication succeeds. Create listeners for these events.

1.  If the authentication method is UNKNOWN, a custom authentication method must be used. In this case, the content provider has arranged for authentication to be done in an out-of-band manner by not using the ActionScript 3.0 APIs. The custom authentication procedure must produce an authentication token that can be passed to the DRMManager.setAuthenticationToken() method.
2.  If authentication fails, your application must return to step 9\. Ensure that your application has a mechanism to handle and limit repeated authentication failures. For example, after three attempts, you display a message to the user indicating the authentication has failed and content cannot be played.
3.  To use the stored token instead of prompting the user to enter credentials, set the token with DRMManager.setAuthenticationToken() method. You then download the license from the license server and play content as in step 8.
4.  (optional) If authentication succeeds, you can capture the authentication token, which is a byte array cached in memory. Get this token with the DRMAuthenticationCompleteEvent.token property. You can store and use the authentication token so that the user does not have to repeatedly enter credentials for this content. The license server determines the valid period of the authentication token.
5.  If authentication succeeds, download the license from the license server:

DRMManager.loadvoucher(_drmContentData_, LoadVoucherSetting.FORCE_REFRESH)

After loading completes, the DRMManager object dispatches DRMStatusEvent.DRM_STATUS. Listen for this event, and when it is dispatched, you can play the content.

1.  Play the video by creating a NetStream object and then calling its play() method:

stream = new NetStream(connection); stream.addEventListener(DRMStatusEvent.DRM _STATUS, drmStatusHandler); stream.addEventListener(DRMErrorEvent.DRM_ERROR, drmErrorHandler); stream.addEventListener(NetStatusEvent.NET_STATUS, netStatusHandler); stream.client = new CustomClient();

video.attachNetStream(stream); stream.play(videoURL);

### DRMContentData and session objects {#drmcontentdata-and-session-objects}

When DRMContentData is created, it will be used as a session object that refers to the Flash Player DRM module. All the DRMManager APIs that receives this DRMContentData will use that particular DRM module. However, there are 2 DRMManager APIs that does not use DRMContentData. They are:

1.  authenticate()
2.  setAuthenticationToken()

Since there is no DRMContentData associated, invoking these DRMManager APIs will use the latest DRM module from the disk. This may become a problem if an update of the DRM module happens in the middle of the application’s DRM workflow. Consider the following scenario:

1.  The application creates a DRMContentData object contentData1, which uses _AdobeCP1_ as the DRM module.
2.  The application invokes the DRMManager.authenticate(contentData1.serverURL,...) method.
3.  The application invokes the DRMManager.loadVoucher(contentData1, ...) method. If an update happens for the DRM module before the application can get to step 2, then the

DRMManager.authenticate() method will end up authenticating using _AdobeCP2_ as the DRM module. The loadVoucher() method in step 3 will fail since it is still using _AdobeCP1_ as the DRM module. The update may have happened due to another application invoking the DRM module update.You can avoid this scenario by invoking the DRM module update on application startup.

### DRM-related events {#drm-related-events}

The runtime dispatches numerous events when an application attempts to play protected content:

• DRMDeviceGroupErrorEvent (AIR only), dispatched by DRMManager

• DRMAuthenticateEvent (AIR only), dispatched by NetStream

• DRMAuthenticationCompleteEvent, dispatched by DRMManager

• DRMAuthenticationErrorEvent, dispatched by DRMManager

• DRMErrorEvent, dispatched by NetStream and DRMManager

• DRMStatusEvent, dispatched by NetStream and DRMManager

• StatusEvent

• NetStatusEvent. See

“Listening for an update event” on page 545

To support content protected by Adobe Access, add event listeners for handling the DRM events.

### Pre-loading vouchers for offline playback {#pre-loading-vouchers-for-offline-playback}

Adobe AIR 1.5 and later

You can preload the vouchers (licenses) required to play content protected by Adobe Access. Pre-loaded vouchers allow users to view the content whether they have an active Internet connection. (The preload process itself requires an Internet connection.) You can use the NetStream class preloadEmbeddedMetadata() method and the DRMManager class to preload vouchers. In AIR 2.0 and later, you can use a DRMContentData object to preload vouchers directly. This technique is preferable because it lets you update the DRMContentData object independent of the content. (The preloadEmbeddedData() method fetches DRMContentData from the content.)

**Using DRMContentData**

Adobe AIR 2.0 and later

The following steps describe the workflow for pre-loading the voucher for a protected media file using a DRMContentData object.

1.  Get the binary metadata for the packaged content. If using the Adobe Access Java Reference Packager, this metadata file is automatically generated with a _.metadata_ extension. You could, for example, download this metadata using the URLLoader class.
2.  Create a DRMContentData object, passing the metadata to the constructor function:

var drmData:DRMContentData = new DRMContentData( metadata );

1.  The rest of the steps are identical to the workflow described in

    “Understanding the protected content workflow” on

    page 529

    .

Using preloadEmbeddedMetadata()

Adobe AIR 1.5 and later

The following steps describe the workflow for pre-loading the voucher for a DRM-protected media file using

preloadEmbeddedMetadata():

1.  Download and store the media file. (DRM metadata can only be pre-loaded from locally stored files.)
2.  Create the NetConnection and NetStream objects, supplying implementations for the onDRMContentData() and

onPlayStatus() callback functions of the NetStream client object.

1.  Create a NetStreamPlayOptions object and set the stream property to the URL of the local media file.
2.  Call the NetStream preloadEmbeddedMetadata(), passing in the NetStreamPlayOptions object identifying the media file to parse.
3.  If the media file contains DRM metadata, then the onDRMContentData() callback function is invoked. The metadata is passed to this function as a DRMContentData object.
4.  Use the DRMContentData object to obtain the voucher using the DRMManager loadVoucher() method.

If the value of the authenticationMethod property of the DRMContentData object is flash.net.drm.AuthenticationMethod.USERNAME_AND_PASSWORD, authenticate the user on the media rights server before loading the voucher. The serverURL and domain properties of the DRMContentData object can be passed to the DRMManager authenticate() method, along with the user’s credentials.

1.  The onPlayStatus() callback function is invoked when file parsing is complete. If the onDRMContentData() function has not been called, the file does not contain the metadata required to obtain a voucher. This missing call also possibly means that Adobe Access does not protect this file.

The following code example for AIR illustrates how to preload a voucher for a local media file:

package

{

import flash.display.Sprite;

import flash.events.DRMAuthenticationCompleteEvent; import flash.events.DRMAuthenticationErrorEvent; import flash.events.DRMErrorEvent;

import flash.ev ents.DRMStatusEvent; import flash.events.NetStatusEvent; import flash.net.NetConnection; import flash.net.NetStream;

import flash.net.NetStreamPlayOptions; import flash.net.drm.AuthenticationMethod; import flash.net.drm.DRMContentData; import flash.net.drm.DRMManager;

import flash.net.drm.LoadVoucherSetting; public class DRMPreloader extends Sprite

{

private var videoURL:String = &quot;app-storage:/video.flv&quot;; private var userName:String = &quot;user&quot;;

private var password:String = &quot;password&quot;; private var preloadConnection:NetConnection; private var preloadStream:NetStream;

private var drmManager:DRMManager = DRMManager.getDRMManager(); private var drmContentData:DRMContentData;

public function DRMPreloader():void { drmManager.addEventListener(

DRMAuthenticationCompleteEvent.AUTHENTICATION_COMPLETE, onAuthenticationComplete);

drmManager.addEventListener(DRMAuthenticationErrorEvent.AUTHENTICATION_ERROR, onAuthenticationError);

drmManager.addEventListener(DRMStatusEvent.DRM_STATUS, onDRMStatus); drmManager.addEventListener(DRMErrorEvent.DRM_ERROR, onDRMError); preloadConnection = new NetConnection(); preloadConnection.addEventListener(NetStatusEvent.NET_STATUS, onConnect); preloadConnection.connect(null);

}

private function onConnect( event:NetStatusEvent ):void

{

preloadMetadata();

}

private function preloadMetadata():void

{

preloadStream = new NetStream( preloadConnection ); preloadStream.client = this;

var options:NetStreamPlayOptions = new NetStreamPlayOptions(); options.streamName = videoURL; preloadStream.preloadEmbeddedData( options );

}

public function onDRMContentData( drmMetadata:DRMContentData ):void

{

drmContentData = drmMetadata;

if ( drmMetadata.authenticationMethod == AuthenticationMethod.USERNAME_AND_PASSWORD )

{

authenticateUser();

}

else

{

getVoucher();

}

}

private function getVoucher():void

{

drmManager.loadVoucher( drmContentData, LoadVoucherSetting.ALLOW_SERVER );

}

private function authenticateUser():void

{

drmManager.authenticate( drmContentData.serverURL, drmContentData.domain, userName, password );

}

private function onAuthenticationError( event:DRMAuthenticationErrorEvent ):void

{

trace( &quot;Authentication error: &quot; + event.errorID + &quot;, &quot; + event.subErrorID );

}

private function onAuthenticationComplete( event:DRMAuthenticationCompleteEvent ):void

{

trace( &quot;Authenticated to: &quot; + event.serverURL + &quot;, domain: &quot; + event.domain ); getVoucher();

}

private function onDRMStatus( event:DRMStatusEvent ):void

{

trace( &quot;DRM Status: &quot; + event.detail);

trace(&quot;--Voucher allows offline playback = &quot; + event.isAvailableOffline ); trace(&quot;--Voucher already cached = &quot; + event.isLocal );

trace(&quot;--Voucher required authentication = &quot; + !event.isAnonymous );

}

private function onDRMError( event:DRMErrorEvent ):void

{

trace( &quot;DRM error event: &quot; + event.errorID + &quot;, &quot; + event.subErrorID + &quot;, &quot; + event.text );

}

public function onPlayStatus( info:Object ):void

{

preloadStream.close();

}

}

}