# Configuration

An example  `config/cbssso.cfc` file.

```cfscript
component {
  public any function configure(){
    return {
      // enable this for cbAuth integration
      "enableCBAuthIntegration": false,
      
      // where cbSSO should redirect when a SSO has an unhandled error
      "errorRedirect": "",
      
      // where cbSSO should redirect after a successful proecess
      // you probably won't need this and will instead want to redirect manually
      // in the CBSSOOnAuthorization interception event
      "successRedirect": "",
      
      // enable this for easy integration with cbSecurity
      // see the "CBSecurity Integration" section for more information
      "enableCBSecurityIntegration": false,
      
      // register your SSO providers
      "providers" : [
        {
          // name is optional, can be used to control the redirect uri
          // with name:     https://your.app.com/cbsso/auth/fbook
          // without name:  https://your.app.com/cbsso/auth/Facebook
          name:         "fbook",
          type:         "FacebookProvider@cbsso",

          // these values are configured with Microsoft and available in your app dashboard
          clientId:     "YOUR-CLIENT-ID",
          clientSecret: "YOUR-CLIENT-SECRET"
        }
      ]
    };  
  }
}
```

The cbSSO module was built to be as easy to use as possible and configuration can be as simple as registering a single provider.

### The enableCBSecurityIntegration Setting

Provided out of the box but disabled by default is the ability to integrate your SSO workflow with the cbSecurity module. In order to fully enable the integration between the two modules you will need to enable this setting and also implement some additional functions in your `UserService`.\
\
See [UserService Additions](https://app.gitbook.com/o/-LA-UVKtPMvMR4t5JRTB/s/bhpL1furbue2wKeid2C7/\~/changes/3/cbsecurity-integration/userservice-additions) for more information.
