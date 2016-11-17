## Dropping file promises {#dropping-file-promises}

Adobe AIR 2 and later

A file promise is a drag-and-drop clipboard format that allows a user to drag a file that does not yet exist out of an AIR application. For example, using file promises, your application could allow a user to drag a proxy icon to a desktop folder. The proxy icon represents a file or some data known to be available at a URL. After the user drops the icon, the runtime downloads the data and writes the file to the drop location.

You can use the URLFilePromise class in an AIR application to drag-and-drop files accessible at a URL. The URLFilePromise implementation is provided in the aircore library as part of the AIR 2 SDK. Use either the aircore.swc or aircore.swf file found in the SDK frameworks/libs/air directory.

Alternately, you can implement your own file promise logic using the IFilePromise interface (which is defined in the runtime flash.desktop package).

File promises are similar in concept to deferred rendering using a data handler function on the clipboard. Use file promises instead of deferred rendering when dragging and dropping files. The deferred rendering technique can lead to undesirable pauses in the drag gesture as the data is generated or downloaded. Use deferred rendering for copy and paste operations (for which file promises are not supported).

Limitations when using file promises

File promises have the following limitations compared to other data formats that you can put in a drag-and-drop clipboard:

• File promises can only be dragged out of an AIR application; they cannot be dropped into an AIR application.

• File promises are not supported on all operating systems. Use the Clipboard.supportsFilePromise property to test whether file promises are supported on the host system. On systems that do not support file promises, you should provide an alternative mechanism for downloading or generating the file data.

• File promises cannot be used with the copy-and-paste clipboard (Clipboard.generalClipboard).

**More Help topics**

[flash.desktop.IFilePromise](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/desktop/IFilePromise.html) [air.desktop.URLFilePromise](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/air/desktop/URLFilePromise.html)

**Dropping remote files**

Adobe AIR 2 and later

Use the URLFilePromise class to create file promise objects representing files or data available at a URL. Add one or more file promise objects to the clipboard using the FILE_PROMISE_LIST clipboard format. In the following example, a single file, available at [http://www.example.com/foo.txt,](http://www.example.com/foo.txt) is downloaded and saved to the drop location as bar.txt. (The remote and the local file names do not have to match.)

if( Clipboard.supportsFilePromise )

{

var filePromise:URLFilePromise = new URLFilePromise(); filePromise.request = new [URLRequest(&quot;http://example.com/foo.txt&quot;);](http://example.com/foo.txt) filePromise.relativePath = &quot;bar.txt&quot;;

var fileList:Array = new Array( filePromise ); var clipboard:Clipboard = new Clipboard();

clipboard.setData( ClipboardFormats.FILE_PROMISE_LIST_FORMAT, fileList ); NativeDragManager.doDrag( dragSource, clipboard );

}

You can allow the user to drag more than one file at a time by adding more file promise objects to the array assigned to the clipboard. You can also specify subdirectories in the relativePath property so that some or all of the files included in the operation are placed in a subfolder relative to the drop location.

The following example illustrates how to initiate a drag operation that includes multiple file promises. In this example, an html page, _article.html_, is put on the clipboard as a file promise, along with its two linked image files. The images are copied into an _images_ subfolder so that the relative links are maintained.

if( Clipboard.supportsFilePromise )

{ //Create the promise objects

var filePromise:URLFilePromise = new URLFilePromise(); filePromise.request = new [URLRequest(&quot;http://example.com/article.html&quot;);](http://example.com/article.html) filePromise.relativePath = &quot;article.html&quot;;

var image1Promise:URLFilePromise = new URLFilePromise();

image1Promise.request = new [URLRequest(&quot;http://example.com/images/img_1.jpg&quot;);](http://example.com/images/img_1.jpg) image1Promise.relativePath = &quot;images/img_1.html&quot;;

var image2Promise:URLFilePromise = new URLFilePromise();

image2Promise.request = new [URLRequest(&quot;http://example.com/images/img_2.jpg&quot;);](http://example.com/images/img_2.jpg) image2Promise.relativePath = &quot;images/img_2.jpg&quot;;

//Put the promise objects onto the clipboard inside an array

var fileList:Array = new Array( filePromise, image1Promise, image2Promise ); var clipboard:Clipboard = new Clipboard();

clipboard.setData( ClipboardFormats.FILE_PROMISE_LIST_FORMAT, fileList );

//Start the drag operation NativeDragManager.doDrag( dragSource, clipboard );

}

### Implementing the IFilePromise interface {#implementing-the-ifilepromise-interface}

Adobe AIR 2 and later

To provide file promises for resources that cannot be accessed using a URLFilePromise object, you can implement the IFilePromise interface in a custom class. The IFilePromise interface defines the methods and properties used by the AIR runtime to access the data to be written to a file once the file promise is dropped.

An IFilePromise implementation passes another object to the AIR runtime that provides the data for the file promise. This object must implement the IDataInput interface, which the AIR runtime uses to read the data. For example, the URLFilePromise class, which implements IFilePromise, uses a URLStream object as the data provider.

AIR can read the data synchronously or asynchronously. The IFilePromise implementation reports which mode of access is supported by returning the appropriate value in the isAsync property. If asynchronous data access is provided, the data provider object must implement the IEventDispatcher interface and dispatch the necessary events, such as open, progress and complete.

You can use a custom class, or one of the following built-in classes, as a data provider for a file promise:

• ByteArray (synchronous)

• FileStream (synchronous or asynchronous)

• Socket (asynchronous)

• URLStream (asynchronous)

To implement the IFilePromise interface, you must provide code for the following functions and properties:

• open():IDataInput — Returns the data provider object from which the data for the promised file is read. The object must implement the IDataInput interface. If the data is provided asynchronously, the object must also implement the IEventDispatcher interface and dispatch the necessary events (see

“Using an asynchronous data

provider in a file promise” on page 627

).

• get relativePath():String — Provides the path, including file name, for the created file. The path is resolved relative to the drop location chosen by the user in the drag-and-drop operation. To make sure that the path uses the proper separator character for the host operating system, use the File.separator constant when specifying paths containing directories. You can add a setter function or use a constructor parameter to allow the path to be set at runtime.

• get isAsync():Boolean — Informs the AIR runtime whether the data provider object provides it’s data asynchronously or synchronously.

• close():void — Called by the runtime when the data is fully read (or an error prevents further reading). You can use this function to cleanup resources.

• reportError( e:ErrorEvent ):void — Called by the runtime when an error reading the data occurs.

All of the IFilePromise methods are called by the runtime during a drag-and-drop operation involving the file promise. Typically, your application logic should not call any of these methods directly.

**Using a synchronous data provider in a file promise**

Adobe AIR 2 and later

The simplest way to implement the IFilePromise interface is to use a synchronous data provider object, such as a ByteArray or a synchronous FileStream. In the following example, a ByteArray object is created, filled with data, and returned when the open() method is called.

package

{

import flash.desktop.IFilePromise; import flash.events.ErrorEvent; import flash.utils.ByteArray; import flash.utils.IDataInput;

public class SynchronousFilePromise implements IFilePromise

{

private const fileSize:int = 5000; //size of file data private var filePath:String = &quot;SynchronousFile.txt&quot;;

public function get relativePath():String

{

return filePath;

}

public function get isAsync():Boolean

{

return false;

}

public function open():IDataInput

{

var fileContents:ByteArray = new ByteArray();

//Create some arbitrary data for the file for( var i:int = 0; i &lt; fileSize; i++ )

{

fileContents.writeUTFBytes( &#039;S&#039; );

}

//Important: the ByteArray is read from the current position fileContents.position = 0;

return fileContents;

}

public function close():void

{

//Nothing needs to be closed in this case.

}

public function reportError(e:ErrorEvent):void

{

trace(&quot;Something went wrong: &quot; + e.errorID + &quot; - &quot; + e.type + &quot;, &quot; + e.text );

}

}

}

In practice, synchronous file promises have limited utility. If the amount of data is small, you could just as easily create a file in a temporary directory and add a normal file list array to the drag-and-drop clipboard. On the other hand, if the amount of data is large or generating the data is computationally expensive, a long synchronous process is necessary. Long synchronous processes can block UI updates for a noticeable amount of time and make your application seem unresponsive. To avoid this problem, you can create an asynchronous data provider driven by a timer.

Using an asynchronous data provider in a file promise

Adobe AIR 2 and later

When you use an asynchronous data provider object, the IFilePromise isAsync property must be true and the object returned by the open() method must implement the IEventDispatcher interface. The runtime listens for several alternative events so that different built-in objects can be used as a data provider. For example, progress events are dispatched by FileStream and URLStream objects, whereas socketData events are dispatched by Socket objects. The runtime listens for the appropriate events from all of these objects.

The following events drive the process of reading the data from the data provider object:

• Event.OPEN — Informs the runtime that the data source is ready.

• ProgressEvent.PROGRESS — Informs the runtime that data is available. The runtime will read the amount of available data from the data provider object.

• ProgressEvent.SOCKET_DATA — Informs the runtime that data is available. The socketData event is dispatched by socket-based objects. For other object types, you should dispatch a progress event. (The runtime listens for both events to detect when data can be read.)

• Event.COMPLETE — Informs the runtime that the data has all been read.

• Event.CLOSE — Informs the runtime that the data has all been read. (The runtime listens for both close and

complete for this purpose.)

• IOErrorEvent.IOERROR — Informs the runtime that an error reading the data has occurred. The runtime aborts file creation and calls the IFilePromise close() method.

• SecurityErrorEvent.SECURITY_ERROR — Informs the runtime that a security error has occurred. The runtime aborts file creation and calls the IFilePromise close() method.

• HTTPStatusEvent.HTTP_STATUS — Used, along with httpResponseStatus, by the runtime to make sure that the data available represents the desired content, rather than an error message (such as a 404 page). Objects based on the HTTP protocol should dispatch this event.

• HTTPStatusEvent.HTTP_RESPONSE_STATUS — Used, along with httpStatus, by the runtime to make sure that the data available represents the desired content. Objects based on the HTTP protocol should dispatch this event.

The data provider should dispatch these events in the following sequence:

1.  open event
2.  progress or socketData events
3.  complete or close event

**_Note:_ **_The built-in objects, FileStream, Socket, and URLStream, dispatch the appropriate events automatically._

The following example creates a file promise using a custom, asynchronous data provider. The data provider class extends ByteArray (for the IDataInput support) and implements the IEventDispatcher interface. At each timer event, the object generates a chunk of data and dispatches a progress event to inform the runtime that the data is available. When enough data has been produced, the object dispatches a complete event.

package

{

import flash.events.Event;

import flash.events.EventDispatcher; import flash.events.IEventDispatcher; import flash.events.ProgressEvent; import flash.events.TimerEvent; import flash.utils.ByteArray;

import flash.utils.Timer;

[Event(name=&quot;open&quot;, type=&quot;flash.events.Event.OPEN&quot;)] [Event(name=&quot;complete&quot;, type=&quot;flash.events.Event.COMPLETE&quot;)] [Event(name=&quot;progress&quot;, type=&quot;flash.events.ProgressEvent&quot;)] [Event(name=&quot;ioError&quot;, type=&quot;flash.events.IOErrorEvent&quot;)] [Event(name=&quot;securityError&quot;, type=&quot;flash.events.SecurityErrorEvent&quot;)]

public class AsyncDataProvider extends ByteArray implements IEventDispatcher

{

private var dispatcher:EventDispatcher = new EventDispatcher(); public var fileSize:int = 0; //The number of characters in the file

private const chunkSize:int = 1000; //Amount of data written per event private var dispatchDataTimer:Timer = new Timer( 100 );

private var opened:Boolean = false;

public function AsyncDataProvider()

{

super();

dispatchDataTimer.addEventListener( TimerEvent.TIMER, generateData );

}

public function begin():void{ dispatchDataTimer.start();

}

public function end():void

{

dispatchDataTimer.stop();

}

private function generateData( event:Event ):void

{

if( !opened )

{

var open:Event = new Event( Event.OPEN ); dispatchEvent( open );

opened = true;

}

else if( position + chunkSize &lt; fileSize )

{

for( var i:int = 0; i &lt;= chunkSize; i++ )

{

writeUTFBytes( &#039;A&#039; );

}

//Set position back to the start of the new data this.position -= chunkSize;

var progress:ProgressEvent =

new ProgressEvent( ProgressEvent.PROGRESS, false, false, bytesAvailable, bytesAvailable + chunkSize);

dispatchEvent( progress )

}

else

{

var complete:Event = new Event( Event.COMPLETE ); dispatchEvent( complete );

}

}

//IEventDispatcher implementation

public function addEventListener(type:String, listener:Function, useCapture:Boolean=false, priority:int=0, useWeakReference:Boolean=false):void

{

dispatcher.addEventListener( type, listener, useCapture, priority, useWeakReference );

}

public function removeEventListener(type:String, listener:Function, useCapture:Boolean=false):void

{

dispatcher.removeEventListener( type, listener, useCapture );

}

public function dispatchEvent(event:Event):Boolean

{

return dispatcher.dispatchEvent( event );

}

public function hasEventListener(type:String):Boolean

{

return dispatcher.hasEventListener( type );

}

public function willTrigger(type:String):Boolean

{

return dispatcher.willTrigger( type );

}

}

}

**_Note:_ **_Because the AsyncDataProvider class in the example extends ByteArray, it cannot also extend EventDispatcher. To implement the IEventDispatcher interface, the class uses an internal EventDispatcher object and forwards the IEventDispatcher method calls to that internal object. You could also extend EventDispatcher and implement IDataInput (or implement both interfaces)._

The asynchronous IFilePromise implementation is almost identical to the synchronous implementation. The main differences are that isAsync returns true and that the open() method returns an asynchronous data object:

package

{

import flash.desktop.IFilePromise; import flash.events.ErrorEvent; import flash.events.EventDispatcher; import flash.utils.IDataInput;

public class AsynchronousFilePromise extends EventDispatcher implements IFilePromise

{

private var fileGenerator:AsyncDataProvider;

private const fileSize:int = 5000; //size of file data private var filePath:String = &quot;AsynchronousFile.txt&quot;;

public function get relativePath():String

{

return filePath;

}

public function get isAsync():Boolean

{

return true;

}

public function open():IDataInput

{

fileGenerator = new AsyncDataProvider(); fileGenerator.fileSize = fileSize; fileGenerator.begin();

return fileGenerator;

}

public function close():void

{

fileGenerator.end();

}

public function reportError(e:ErrorEvent):void

{

trace(&quot;Something went wrong: &quot; + e.errorID + &quot; - &quot; + e.type + &quot;, &quot; + e.text );

}

}

}