# MircosoftSAMLProvider

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

