## Out-of-band Licenses {#out-of-band-licenses}

Flash Player 11 and later, Adobe AIR 3.0 and later

Licenses can also be obtained out-of-band (without contacting a Adobe Access License Server) by storing the voucher (license) on disk and in memory using the storeVoucher method.

To play encrypted videos in Flash Player and AIR, the respective runtime needs to obtain the DRM voucher for that video. The DRM voucher contains the video’s decryption key and is generated by the Adobe Access License Server that the customer has deployed.

The Flash Player/AIR runtime generally obtains this voucher by sending a voucher request to the Adobe Access License Server indicated in the video’s DRM metadata (DRMContentData class). The Flash/AIR application can trigger this license request by calling the DRMManager.loadVoucher()method. Or, the Flash Player/AIR runtime will automatically request a license at the start of the encrypted video playback if there is no license for the content on disk or in memory. In either case, the Flash/AIR application’s performance gets impacted by communicating with the Adobe Access License Server.

DRMManager.storeVoucher() allows the Flash/AIR application to send DRM vouchers that it has obtained out-of- band to the Flash Player/AIR runtime. The runtime can then skip the license request process and use the forwarded vouchers for playing encrypted videos. The DRM voucher still needs to be generated by the Adobe Access License Server before it can be obtained out-of-band. However, you have the option of hosting the vouchers on any HTTP server, instead of a public-facing Adobe Access license server.

DRMManager.storeVoucher() is also used to support DRM voucher sharing between multiple devices. In Adobe Access 3.0, this feature is referred to as “Domain Support”. If your deployment supports this use case, you can register multiple machines to a device group using the DRMManager.addToDeviceGroup() method. If there is a machine with a valid domain-bound voucher for a given content, the AIR application can then extract the serialized DRM vouchers using the DRMVoucher.toByteArray() method and on your other machines you can import the vouchers using the DRMManager.storeVoucher()method.

**Device registration**

The DRM vouchers are bound to the end user’s machine. Hence Flash /AIR applications will need a unique ID for the user’s machine to refer to the right serialized DRM voucher object. The following scenario depicts a device registration process:

Assuming that you have performed the following tasks:

• You have set up the Adobe Access Server SDK.

• You have set up an HTTP server for obtaining pre-generated licenses.

• You have created a Flash application to view the protected content. The device registration phase involves the following actions:

1.  The Flash application creates a randomly generated ID.
2.  The Flash application invokes the DRMManager.authenticate() method. The application must include the randomly generated ID in the authentication request. For instance, include the ID in the username field.
3.  The action mentioned in Step 2 will result in Adobe Access sending an authentication request to the customer’s server. This request includes the device certificate.
    1.  The server extracts the device certificate and the generated ID from the request and stores.
    2.  The customer sub-system pre-generates licenses for this device certificate, stores them and grants access to them in a way that associates them with the generated ID.
4.  The server responds to the request with a “success” message.
5.  The Flash application stores the generated ID locally in a Local Shared Object (LSO).

After the device registration, the Flash application uses the generated ID in the same way as it would have used the device ID in the previous scheme:

1.  The Flash application will try to locate the generated ID in LSO.
2.  If the generated ID is found, the Flash application will use the generated ID while downloading the pre-generated licenses. The Flash application will send the licenses to the Adobe Access client for consumption using the DRMManager.storeVoucher() method.
3.  If the generated ID is not found, the Flash application will go through the device registration procedure.

### Factory reset {#factory-reset}

When the user of the device invokes the factory reset option, the device certificate will be purged. To continue playing the protected content, the Flash application will need to go through the device registration procedure again. If the Flash application sends an outdated pre-generated license, the Adobe Access client will reject it since the license was encrypted for an older device ID.