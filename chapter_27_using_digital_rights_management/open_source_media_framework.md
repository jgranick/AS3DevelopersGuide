## Open Source Media Framework {#open-source-media-framework}

Open Source Media Framework (OSMF) is an ActionScript-based framework that gives you complete flexibility and control in creating your own rich media experiences. For more information on OSMF, visit the [OSMF Developer Site](http://www.osmf.org/developers.html).

### Workflow for playing protected content {#workflow-for-playing-protected-content}

1.  Create a MediaPlayer instance.

player = new MediaPlayer();

1.  Register MediaPlayerCapabilityChangeEvent.HAS_DRM_CHANGE event to the player. This event will be dispatched if the content is DRM protected.

player.addEventListener(MediaPlayerCapabilityChangeEvent.HAS_DRM_CHANGE, onDRMCapabilityChange);

1.  In the event handler, obtain the DRMTrait instance. DRMTrait is the interface through which you invoke DRM- related methods, such as authenticate(). When loading a DRM-protected content, OSMF performs the DRM validating actions and dispatches state events. Add a DRMEvent.DRM_STATE_CHANGE event handler to the DRMTrait.

private function onDRMCapabilityChange

(event :MediaPlayerCapabilityChangeEvent) :void

{

if (event.type == MediaPlayerCapabilityChangeEvent.HAS_DRM_CHANGE &amp;&amp; event.enabled)

{

drmTrait = player.media.getTrait(MediaTraitType.DRM) as DRMTrait; drmTrait.addEventListener

(DRMEvent.DRM_STATE_CHANGE, onDRMStateChange);

}

}

1.  Handle the DRM events in the onDRMStateChange() method.

private function onDRMStateChange(event :DRMEvent) :void

{

trace ( &quot;DRMState: &quot;,event.drmState); switch(event.drmState)

{

case DRMState.AUTHENTICATION_NEEDED:

// Identity-based content

var authPopup :AuthWindow = AuthWindow.create(_parentWin); authPopup.serverURL = event.serverURL; authPopup.addEventListener(&quot;dismiss&quot;, function () :void {

trace (&quot;Authentication dismissed&quot;); if(_drmTrait != null)

{

}

});

//Ignore authentication. Just

//try to acquire a license.

_drmTrait.authenticate(null, null);

authPopup.addEventListener(&quot;authenticate&quot;,

function (event :AuthWindowEvent) :void { if(_drmTrait != null)

{

_drmTrait.authenticate(event.username, event.password);

}

});

authPopup.show(); break;

case DRMState.AUTHENTICATING:

//Display any authentication message.

trace(&quot;Authenticating...&quot;); break;

case DRMState.AUTHENTICATION_COMPLETE:

// Start to retrieve voucher and playback.

// You can display the voucher information at this point. if(event.token)

// You just received the authentication token.

{

trace(&quot;Authentication success. Token: \n&quot;, event.token);

}

else

// You have got the voucher.

{

trace(&quot;DRM License:&quot;); trace(&quot;Playback window period: &quot;,

!isNaN(event.period) ? event.period == 0 ? &quot;&lt;unlimited&gt;&quot; : event.period : &quot;&lt;none&gt;&quot;);

trace(&quot;Playback window end date: &quot;,

event.endDate != null ? event.endDate : &quot;&lt;none&gt;&quot;); trace(&quot;Playback window start date: &quot;,

event.startDate != null ? event.startDate : &quot;&lt;none&gt;&quot;);

}

break;

case DRMState.AUTHENTICATION_ERROR:

trace (&quot;DRM Error:&quot;, event.mediaError.errorID + &quot;[&quot; + DRMErrorEventRef.getDRMErrorMnemonic (event.mediaError.errorID) + &quot;]&quot;);

//Stop everything. player.media = null; break;

case DRMState.DRM_SYSTEM_UPDATING: Logger.log(&quot;Downloading DRM module...&quot;); break;

case DRMState.UNINITIALIZED:

break;

}

}