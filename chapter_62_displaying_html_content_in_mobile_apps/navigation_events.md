## Navigation events {#navigation-events}

When a user clicks a link in the HTML, the StageWebView object dispatches a locationChanging event. You can call the preventDefault() method of the event object to stop the navigation. Otherwise, the StageWebView object navigates to the new page and dispatches a locationChange event. When the page load has finished, the StageWebView dispatches a complete event.

A locationChanging event is dispatched on every HTML redirect. The locationChange and complete events are dispatched at the appropriate time.

On iOS, a locationChanging event is dispatched before a locationChange event, except for the first loadURL() or loadString() methods. A locationChange event is also dispatched for navigational changes through iFrames and Frames.

The following example illustrates how you can prevent a location change and open the new page in the system browser instead.

package {

import flash.display.MovieClip; import flash.media.StageWebView;

import flash.events.LocationChangeEvent; import flash.geom.Rectangle;

import flash.net.navigateToURL; import flash.net.URLRequest;

public class StageWebViewNavEvents extends MovieClip{ var webView:StageWebView = new StageWebView();

public function StageWebViewNavEvents() { webView.stage = this.stage;

webView.viewPort = new Rectangle( 0, 0, stage.stageWidth, stage.stageHeight ); webView.addEventListener( LocationChangeEvent.LOCATION_CHANGING, onLocationChanging );

webView.loadURL( [&quot;http://www.adobe.com&quot;](http://www.adobe.com/) );

}

private function onLocationChanging( event:LocationChangeEvent ):void

{

event.preventDefault();

navigateToURL( new URLRequest( event.location ) );

}

}

}