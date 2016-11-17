## Implementing the IURIDereferencer interface {#implementing-the-iuridereferencer-interface}

Adobe AIR 1.5 and later

To validate an XML signature, you must provide an implementation of the IURIDereferencer interface. The implementation is responsible for resolving the URIs within the Reference elements of an XML signature document and returning the data so that the digest can be computed. The computed digest is compared with the digest in the signature to determine if the referenced data has been altered since the signature was created.

**_Note:_ **_HTML-based AIR applications must import a SWF library containing an ActionScript implementation in order to validate XML signatures. The IURIDereferencer interface cannot be implemented in JavaScript._

The IURIDerefencer interface has a single method, dereference(uri:String), that must be implemented. The XMLSignatureValidator object calls this method for each reference in the signature. The method must return the data in a ByteArray object.

In most cases, you will also need to add properties or methods that allow your dereferencer object to locate the referenced data. For example, if the signed data is located in the same document as the signature, you could add a member variable that provides a reference to the XML document. The dereference() method can then use this variable, along with the URI, to locate the referenced data. Likewise, if the signed data is located in a directory of the local file system, the dereference() method might need a property providing the path to that directory in order to resolve the referenced files.

The XMLSignatureValidator relies entirely on the dereferencer for interpreting URI strings. The standard rules for dereferencing URIs are given in the section 4.3.3 of the W3C Recommendation for XML Signature Syntax and Processing.

**Dereferencing URIs in enveloped signatures**

Adobe AIR 1.5 and later

When an enveloped XML signature is generated, the signature elements are inserted into the signed data. For example, if you signed the following message using an enveloped signature structure:

&lt;message&gt;

&lt;data&gt;...&lt;/data&gt;

&lt;/message&gt;

The resulting signed document will look like this:

&lt;message&gt;

&lt;data&gt;...&lt;/data&gt;

&lt;Signature [xmlns=&quot;http://www.w3.org/2000/09/xmldsig#&quot;&gt;](http://www.w3.org/2000/09/xmldsig)

&lt;SignedInfo&gt;

&lt;CanonicalizationMethod [Algorithm=&quot;http://www.w3.org/TR/2001/REC-xml-c14n-](http://www.w3.org/TR/2001/REC-xml-c14n-)

20010315&quot;/&gt;

&lt;SignatureMethod [Algorithm=&quot;http://www.w3.org/2000/09/xmldsig#rsa-sha1&quot;/&gt;](http://www.w3.org/2000/09/xmldsig#rsa-sha1)

&lt;Reference URI=&quot;&quot;&gt;

&lt;Transforms&gt;

&lt;Transform [Algorithm=&quot;http://www.w3.org/2000/09/xmldsig#enveloped-](http://www.w3.org/2000/09/xmldsig#enveloped-)

signature&quot;/&gt;

&lt;/Transforms&gt;

&lt;DigestMethod [Algorithm=&quot;http://www.w3.org/2001/04/xmlenc#sha256&quot;/&gt;](http://www.w3.org/2001/04/xmlenc#sha256)

&lt;DigestValue&gt;yv6...Z0Y=&lt;/DigestValue&gt;

&lt;/Reference&gt;

&lt;/SignedInfo&gt;

&lt;SignatureValue&gt;cCY...LQ==&lt;/SignatureValue&gt;

&lt;KeyInfo&gt;

&lt;X509Data&gt;

&lt;X509Certificate&gt;MII...4e&lt;/X509Certificate&gt;

&lt;/X509Data&gt;

&lt;/KeyInfo&gt;

&lt;/Signature&gt;

&lt;/message&gt;

Notice that the signature contains a single Reference element with an empty string as its URI. An empty string in this context refers to the root of the document.

Also notice that the transform algorithm specifies that an enveloped signature transform has been applied. When an enveloped signature transform has been applied, the XMLSignatureValidator automatically removes the signature from the document before computing the digest. This means that the dereferencer does not need to remove the Signature element when returning the data.

The following example illustrates a dereferencer for enveloped signatures:

package

{

import flash.events.ErrorEvent; import flash.events.EventDispatcher;

import flash.security.IURIDereferencer; import flash.utils.ByteArray;

import flash.utils.IDataInput;

public class EnvelopedDereferencer

extends EventDispatcher implements IURIDereferencer

{

private var signedMessage:XML;

public function EnvelopedDereferencer( signedMessage:XML )

{

this.signedMessage = signedMessage;

}

public function dereference( uri:String ):IDataInput

{

try

{

if( uri.length != 0 )

{

throw( new Error(&quot;Unsupported signature type.&quot;) );

}

var data:ByteArray = new ByteArray(); data.writeUTFBytes( signedMessage.toXMLString() ); data.position = 0;

}

catch (e:Error)

{

var error:ErrorEvent =

new ErrorEvent(&quot;Ref error &quot; + uri + &quot; &quot;, false, false, e.message); this.dispatchEvent(error);

data = null;

throw new Error(&quot;Reference not resolvable: &quot; + uri + &quot;, &quot; + e.message);

}

finally

{

return data;

}

}

}

}

This dereferencer class uses a constructor function with a parameter, signedMessage, to make the enveloped signature document available to the dereference() method. Since the reference in an enveloped signature always refers to the root of the signed data, the dereferencer() method writes the document into a byte array and returns it.

### Dereferencing URIs in enveloping and detached signatures {#dereferencing-uris-in-enveloping-and-detached-signatures}

Adobe AIR 1.5 and later

When the signed data is located in the same document as the signature itself, the URIs in the references typically use XPath or XPointer syntax to address the elements that are signed. The W3C Recommendation for XML Signature Syntax and Processing only recommends this syntax, so you should base your implementation on the signatures you expect to encounter (and add sufficient error checking to gracefully handle unsupported syntax).

The signature of an AIR application is an example of an enveloping signature. The files in the application are listed in a Manifest element. The Manifest element is addressed in the Reference URI attribute using the string, “#PackageContents”, which refers to the Id of the Manifest element:

&lt;Signature [xmlns=&quot;http://www.w3.org/2000/09/xmldsig#&quot;](http://www.w3.org/2000/09/xmldsig) Id=&quot;PackageSignature&quot;&gt;

&lt;SignedInfo&gt;

&lt;CanonicalizationMethod [Algorithm=&quot;http://www.w3.org/TR/2001/REC-xml-c14n-](http://www.w3.org/TR/2001/REC-xml-c14n-) 20010315&quot;/&gt;

&lt;SignatureMethod [Algorithm=&quot;http://www.w3.org/TR/xmldsig-core#rsa-sha1&quot;/&gt;](http://www.w3.org/TR/xmldsig-core#rsa-sha1)

&lt;Reference URI=&quot;#PackageContents&quot;&gt;

&lt;Transforms&gt;

&lt;Transform [Algorithm=&quot;http://www.w3.org/TR/2001/REC-xml-c14n-20010315&quot;/&gt;](http://www.w3.org/TR/2001/REC-xml-c14n-20010315)

&lt;/Transforms&gt;

&lt;DigestMethod [Algorithm=&quot;http://www.w3.org/2001/04/xmlenc#sha256&quot;/&gt;](http://www.w3.org/2001/04/xmlenc#sha256)

&lt;DigestValue&gt;ZMGqQdaRKQc1HirIRsDpeBDlaElS+pPotdziIAyAYDk=&lt;/DigestValue&gt;

&lt;/Reference&gt;

&lt;/SignedInfo&gt;

&lt;SignatureValue Id=&quot;PackageSignatureValue&quot;&gt;cQK...7Zg==&lt;/SignatureValue&gt;

&lt;KeyInfo&gt;

&lt;X509Data&gt;

&lt;X509Certificate&gt;MII...T4e&lt;/X509Certificate&gt;

&lt;/X509Data&gt;

&lt;/KeyInfo&gt;

&lt;Object&gt;

&lt;Manifest Id=&quot;PackageContents&quot;&gt;

&lt;Reference URI=&quot;mimetype&quot;&gt;

&lt;DigestMethod [Algorithm=&quot;http://www.w3.org/2001/04/xmlenc#sha256&quot;&gt;](http://www.w3.org/2001/04/xmlenc#sha256)

&lt;/DigestMethod&gt;

&lt;DigestValue&gt;0/oCb84THKMagtI0Dy0KogEu92TegdesqRr/clXct1c=&lt;/DigestValue&gt;

&lt;/Reference&gt;

&lt;Reference URI=&quot;META-INF/AIR/application.xml&quot;&gt;

&lt;DigestMethod [Algorithm=&quot;http://www.w3.org/2001/04/xmlenc#sha256&quot;&gt;](http://www.w3.org/2001/04/xmlenc#sha256)

&lt;/DigestMethod&gt;

&lt;DigestValue&gt;P9MqtqSdqcqnFgeoHCJysLQu4PmbUW2JdAnc1WLq8h4=&lt;/DigestValue&gt;

&lt;/Reference&gt;

&lt;Reference URI=&quot;XMLSignatureValidation.swf&quot;&gt;

&lt;DigestMethod [Algorithm=&quot;http://www.w3.org/2001/04/xmlenc#sha256&quot;&gt;](http://www.w3.org/2001/04/xmlenc#sha256)

&lt;/DigestMethod&gt;

&lt;DigestValue&gt;OliRHRAgc9qt3Dk0m0Bi53Ur5ur3fAweIFwju74rFgE=&lt;/DigestValue&gt;

&lt;/Reference&gt;

&lt;/Manifest&gt;

&lt;/Object&gt;

&lt;/Signature&gt;

A dereferencer for validating this signature must take the URI string containing, &quot;#PackageContents&quot; from the Reference element, and return the Manifest element in a ByteArray object. The “#” symbol refers to the value of an element Id attribute.

The following example implements a dereferencer for validating AIR application signatures. The implementation is kept simple by relying on the known structure of an AIR signature. A general-purpose dereferencer could be significantly more complex.

package

{

import flash.events.ErrorEvent;

import flash.security.IURIDereferencer; import flash.utils.ByteArray;

import flash.utils.IDataInput;

public class AIRSignatureDereferencer implements IURIDereferencer { private const XML_SIG_NS:Namespace =

new Namespace( [&quot;http://www.w3.org/2000/09/xmldsig#&quot;](http://www.w3.org/2000/09/xmldsig) ); private var airSignature:XML;

public function AIRSignatureDereferencer( airSignature:XML ) { this.airSignature = airSignature;

}

public function dereference( uri:String ):IDataInput { var data:ByteArray = null;

try

{

if( uri != &quot;#PackageContents&quot; )

{

throw( new Error(&quot;Unsupported signature type.&quot;) );

}

var manifest:XMLList = airSignature.XML_SIG_NS::Object.XML_SIG_NS::Manifest;

data = new ByteArray();

data.writeUTFBytes( manifest.toXMLString()); data.position = 0;

}

catch (e:Error)

{

data = null;

throw new Error(&quot;Reference not resolvable: &quot; + uri + &quot;, &quot; + e.message);

}

finally

{

return data;

}

}

}

}

When you verify this type of signature, only the data in the Manifest element is validated. The actual files in the package are not checked at all. To check the package files for tampering, you must read the files, compute the SHA256 digest and compare the result to digest recorded in the manifest. The XMLSignatureValidator does not automatically check such secondary references.

**_Note:_ **_This example is provided only to illustrate the signature validation process. There is little use in an AIR application validating its own signature. If the application has already been tampered with, the tampering agent could simply remove the validation check._

**Computing digest values for external resources**

Adobe AIR 1.5 and later

AIR does not include built-in functions for computing SHA256 digests, but the Flex SDK does include a SHA256 utility class. The SDK also includes the Base64 encoder utility class that is helpful for comparing the computed digest to the digest stored in a signature.

The following example function reads and validates the files in an AIR package manifest:

import mx.utils.Base64Encoder; import mx.utils.SHA256;

private function verifyManifest( sigFile:File, manifest:XML ):Boolean

{

var result:Boolean = true; var message:String = &#039;&#039;;

var nameSpace:Namespace = manifest.namespace();

if( manifest.nameSpace::Reference.length() &lt;= 0 )

{

result = false;

message = &quot;Nothing to validate.&quot;;

}

for each (var reference:XML in manifest.nameSpace::Reference)

{

var file:File = sigFile.parent.parent.resolvePath( reference.@URI ); var stream:FileStream = new FileStream();

stream.open(file, FileMode.READ);

var fileData:ByteArray = new ByteArray(); stream.readBytes( fileData, 0, stream.bytesAvailable );

var digestHex:String = SHA256.computeDigest( fileData );

//Convert hexidecimal string to byte array var digest:ByteArray = new ByteArray();

for( var c:int = 0; c &lt; digestHex.length; c += 2 ){

var byteChar:String = digestHex.charAt(c) + digestHex.charAt(c+1); digest.writeByte( parseInt( byteChar, 16 ));

}

digest.position = 0;

var base64Encoder:Base64Encoder = new Base64Encoder(); base64Encoder.insertNewLines = false; base64Encoder.encodeBytes( digest, 0, digest.bytesAvailable ); var digestBase64:String = base64Encoder.toString();

if( digestBase64 == reference.nameSpace::DigestValue )

{

result = result &amp;&amp; true;

message += &quot; &quot; + reference.@URI + &quot; verified.\n&quot;;

}

else

{

result = false;

message += &quot; ---- &quot; + reference.@URI + &quot; has been modified!\n&quot;;

}

base64Encoder.reset();

}

trace( message ); return result;

}

The function loops through all the references in the Manifest element. For each reference, the SHA256 digest is computed, encoded in base64 format, and compared to the digest in the manifest. The URIs in an AIR package refer to paths relative to the application directory. The paths are resolved based on the location of the signature file, which is always in the META-INF subdirectory within the application directory. Note that the Flex SHA256 class returns the digest as a string of hexadecimal characters. This string must be converted into a ByteArray containing the bytes represented by the hexadecimal string.

To use the mx.utils.SHA256 and Base64Encoder classes in Flash CS4, you can either locate and copy these classes into your application development directory or compile a library SWF containing the classes using the Flex SDK.

### Dereferencing URIs in detached signatures referencing external data {#dereferencing-uris-in-detached-signatures-referencing-external-data}

Adobe AIR 1.5 and later

When a URI refers to an external resource, the data must be accessed and loaded into a ByteArray object. If the URI contains an absolute URL, then it is simply a matter of reading a file or requesting a URL. If, as is probably the more common case, the URI contains to a relative path, then your IURIDereferencer implementation must include a way to resolve the paths to the signed files.

The following example uses a File object initialized when the dereferencer instance is constructed as the base for resolving signed files.

package

{

import flash.events.ErrorEvent; import flash.events.EventDispatcher; import flash.filesystem.File;

import flash.filesystem.FileMode; import flash.filesystem.FileStream; import flash.security.IURIDereferencer; import flash.utils.ByteArray;

import flash.utils.IDataInput;

public class RelativeFileDereferencer

extends EventDispatcher implements IURIDereferencer

{

private var base:File;

public function RelativeFileDereferencer( base:File )

{

this.base = base;

}

public function dereference( uri:String ):IDataInput

{

var data:ByteArray = null; try{

var referent:File = this.base.resolvePath( uri ); var refStream:FileStream = new FileStream();

data = new ByteArray();

refStream.open( referent, FileMode.READ ); refStream.readBytes( data, 0, data.bytesAvailable );

} catch ( e:Error ) { data = null;

throw new Error(&quot;Reference not resolvable: &quot; + referent.nativePath + &quot;, &quot; +

e.message );

} finally {

return data;

}

}

}

}

The dereference() function simply locates the file addressed by the reference URI, loads the file contents into a byte array, and returns the ByteArray object.

**_Note:_ **_Before validating remote external references, consider whether your application could be vulnerable to a “phone home” or similar type of attack by a maliciously constructed signature document._