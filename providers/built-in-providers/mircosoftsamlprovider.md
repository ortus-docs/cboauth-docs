# MicrosoftSAMLProvider

The MicrosoftSAMLProvider gives you the ability to integrate with Microsoft's Entra single sign-on service. You can read more about [Microsoft's Entra AuthNRequest workflow here.](https://learn.microsoft.com/en-us/entra/identity-platform/single-sign-on-saml-protocol)

### Example Configuration

```cfscript
component {
  public any function configure(){
    return {
      "providers" : [
        {
          // name is optional, can be used to control the redirect uri
          // with name:     https://your.app.com/cbsso/auth/entra
          // without name:  https://your.app.com/cbsso/auth/MicrosoftSAMLProvider
          name:         "entra",
          type:         "MicrosoftSAMLProvider@cbsso",

          // these values are configured with Microsoft and available in your app dashboard
          clientId:     "YOUR-CLIENT-ID",
          clientSecret: "YOUR-CLIENT-SECRET",
          authEndpoint: "https://login.microsoftonline.com/YOUR-TENANT-ID/saml2",
          expectedIssuer: "https://sts.windows.net/YOUR-TENANT-ID/",
          federationMetadataURL: "https://login.microsoftonline.com/YOUR-TENANT-ID/federationmetadata/2007-06/federationmetadata.xml"
        }
      ]
    };  
  }
}
```

### Validation and replay protection

`MicrosoftSAMLProvider` accepts a response only after it has passed the following checks:

* The response and assertion issuers match `expectedIssuer`.
* The assertion audience matches the configured `clientId`.
* The assertion's bearer confirmation is addressed to the configured ACS URL. Set `redirectUri` when the
  default `/cbsso/auth/:providerName` URL is not used.
* The assertion is signed by a certificate from the federation metadata, and its signature reference is
  bound to the assertion whose claims will be read.
* The response and the accepted bearer subject confirmation echo the AuthNRequest ID that started the flow.
* The assertion has a valid time window. A 60-second clock-skew allowance is applied.
* The response contains exactly one assertion. Encrypted assertions are not supported because cbSSO cannot
  decrypt them.

Federation metadata is fetched with 10-second connection and request timeouts. The provider cannot validate
responses until the metadata has been fetched successfully and at least one IdP signing certificate has been
loaded.

Identity is extracted only from the assertion whose signature was verified. Attributes or `NameID` values
added elsewhere in the response are not treated as user data. XML documents with a `DOCTYPE`, non-XML input,
or a decoded response larger than 1 MiB are rejected before normal response processing.

Each AuthNRequest ID is stored in the CacheBox cache selected by `samlRequestCacheName` and is consumed after
successful validation. This prevents a valid response from being replayed and does not require session
management. See [SAML request replay cache](../../usage/configuration.md#saml-request-replay-cache) for the
single-node default and distributed-cache configuration required for clustered deployments.

### Additional Server Configuration

If you are using the `MicrosoftSAMLProvider` you will need to add some java libraries to your server.\
\
If using a CommandBox `server.json` you can do that like so

````
```jsonc
{
    "app":{
        // add this line to ensure the java library is loaded at the appropriate level
        "libDirs":"modules/cbsso/lib"
    }
}
```
````

