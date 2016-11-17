## History {#history}

As a user clicks links in the content displayed in a StageWebView object, the control saves the backwards and forwards history stacks. The following example illustrates how to navigate through the two history stacks. The example uses the Back and Search soft keys.

package {

import flash.display.MovieClip; import flash.media.StageWebView; import flash.geom.Rectangle; import flash.events.KeyboardEvent; import flash.ui.Keyboard;

public class StageWebViewExample extends MovieClip{ var webView:StageWebView = new StageWebView(); public function StageWebViewExample()

{

webView.stage = this.stage;

webView.viewPort = new Rectangle( 0, 0, stage.stageWidth, stage.stageHeight ); webView.loadURL( [&quot;http://www.adobe.com&quot;](http://www.adobe.com/) );

stage.addEventListener( KeyboardEvent.KEY_DOWN, onKey );

}

private function onKey( event:KeyboardEvent ):void

{

if( event.keyCode == Keyboard.BACK &amp;&amp; webView.isHistoryBackEnabled )

{

trace(&quot;back&quot;); webView.historyBack(); event.preventDefault();

}

if( event.keyCode == Keyboard.SEARCH &amp;&amp; webView.isHistoryForwardEnabled )

{

trace(&quot;forward&quot;); webView.historyForward();

}

}

}

}