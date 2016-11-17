## Bitmap capture {#bitmap-capture}

A StageWebView object is rendered above all display list content. You cannot add a content above a StageWebView object. For example, you cannot expand a drop-down over the StageWebView content. To solve this issue, capture a snapshot of the StageWebView. Then, hide the StageWebView and add the bitmap snapshot instead.

The following example illustrates how to capture the snapshot of a StageWebView object using the drawViewPortToBitmapData method. It hides the StageWebView object by setting the stage to null. After the web page is fully loaded, it calls a function that captures the bitmap and displays it. When you run, the code displays two labels, Google and Facebook. Clicking the label captures the corresponding web page and displays it as a snapshot on the stage.

package

{

import flash.display.Bitmap; import flash.display.BitmapData; import flash.display.Sprite; import flash.events.*;

import flash.geom.Rectangle; import flash.media.StageWebView; import flash.net.*;

import flash.text.TextField;

public class stagewebview extends Sprite

{

public var webView:StageWebView=new StageWebView(); public var textGoogle:TextField=new TextField(); public var textFacebook:TextField=new TextField(); public function stagewebview()

{

textGoogle.htmlText=&quot;&lt;b&gt;Google&lt;/b&gt;&quot;; textGoogle.x=300;

textGoogle.y=-80; addChild(textGoogle);

textFacebook.htmlText=&quot;&lt;b&gt;Facebook&lt;/b&gt;&quot;; textFacebook.x=0;

textFacebook.y=-80; addChild(textFacebook);

textGoogle.addEventListener(MouseEvent.CLICK,goGoogle); textFacebook.addEventListener(MouseEvent.CLICK,goFaceBook); webView.stage = this.stage;

webView.viewPort = new Rectangle(0, 0, stage.stageWidth, stage.stageHeight);

}

public function goGoogle(e:Event):void

{

[webView.loadURL(&quot;http://www.google.com&quot;);](http://www.google.com/) webView.stage = null; webView.addEventListener(Event.COMPLETE,handleLoad);

}

public function goFaceBook(e:Event):void

{

[webView.loadURL(&quot;http://www.facebook.com&quot;);](http://www.facebook.com/) webView.stage = null; webView.addEventListener(Event.COMPLETE,handleLoad);

}

public function handleLoad(e:Event):void

{

var bitmapData:BitmapData = new BitmapData(webView.viewPort.width, webView.viewPort.height);

webView.drawViewPortToBitmapData(bitmapData); var webViewBitmap:Bitmap=new Bitmap(bitmapData); addChild(webViewBitmap);

}

}

}