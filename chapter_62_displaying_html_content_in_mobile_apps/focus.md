## Focus {#focus}

Even though it is not a display object, the StageWebView class contains members that allow you to manage the focus transitions into and out of the control.

When the StageWebView object gains focus, it dispatches a focusIn event. You use this event to manage the focus elements in your application, if necessary.

When the StageWebView relinquishes the focus, it dispatches a focusOut event. A StageWebView instance can relinquish focus when a user “tabs” past the first or last control on the web page using a device trackball or directional arrows. The direction property of the event object lets you know whether the focus flow is rising up past the top of the page or down through the bottom of the page. Use this information to assign focus to the appropriate display object above or below the StageWebView.

On iOS, focus cannot be set programmatically. StageWebView dispatches focusIn and focusOut events with the direction property of FocusEvent set to none. If the user touches inside the StageWebView, the focusIn event is dispatched. If the user touches outside the StageWebView, the focusOut event is dispatched.

The following example illustrates how focus passes from the StageWebView object to Flash display objects:

package {

import flash.display.MovieClip; import flash.media.StageWebView; import flash.geom.Rectangle; import flash.events.KeyboardEvent; import flash.ui.Keyboard;

import flash.text.TextField; import flash.text.TextFieldType; import flash.events.FocusEvent;

import flash.display.FocusDirection; import flash.events.LocationChangeEvent;

public class StageWebViewFocusEvents extends MovieClip{ var webView:StageWebView = new StageWebView();

var topControl:TextField = new TextField(); var bottomControl:TextField = new TextField();

public function StageWebViewFocusEvents()

{

trace(&quot;Starting&quot;);

topControl.type = TextFieldType.INPUT; addChild( topControl ); topControl.height = 60; topControl.width = stage.stageWidth; topControl.background = true;

topControl.text = &quot;One control on top.&quot;; topControl.addEventListener( FocusEvent.FOCUS_IN, flashFocusIn ); topControl.addEventListener( FocusEvent.FOCUS_OUT, flashFocusOut );

- 120 );

webView.stage = this.stage;

webView.viewPort = new Rectangle( 0, 60, stage.stageWidth, stage.stageHeight

webView.addEventListener( FocusEvent.FOCUS_IN, webFocusIn ); webView.addEventListener(FocusEvent.FOCUS_OUT, webFocusOut ); webView.addEventListener(LocationChangeEvent.LOCATION_CHANGING,

function( event:LocationChangeEvent ):void

{

event.preventDefault();

} );

webView.loadString(&quot;&lt;form action=&#039;#&#039;&gt;&lt;input/&gt;&lt;input/&gt;&lt;input/&gt;&lt;/form&gt;&quot;); webView.assignFocus();

bottomControl.type = TextFieldType.INPUT; addChild( bottomControl ); bottomControl.y = stage.stageHeight - 60; bottomControl.height = 60; bottomControl.width = stage.stageWidth; bottomControl.background = true;

bottomControl.text = &quot;One control on the bottom.&quot;; bottomControl.addEventListener( FocusEvent.FOCUS_IN, flashFocusIn ); bottomControl.addEventListener( FocusEvent.FOCUS_OUT, flashFocusOut );}

private function webFocusIn( event:FocusEvent ):void

{

trace(&quot;Web focus in&quot;);

}

private function webFocusOut( event:FocusEvent ):void

{

trace(&quot;Web focus out: &quot; + event.direction); if( event.direction == FocusDirection.TOP )

{

stage.focus = topControl;

}

else

{

stage.focus = bottomControl;

}

}

private function flashFocusIn( event:FocusEvent ):void

{

trace(&quot;Flash focus in&quot;);

var textfield:TextField = event.target as TextField; textfield.backgroundColor = 0xff5566;

}

private function flashFocusOut( event:FocusEvent ):void

{

trace(&quot;Flash focus out&quot;);

var textfield:TextField = event.target as TextField; textfield.backgroundColor = 0xffffff;

}

}

}