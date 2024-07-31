# GoogleProvider

Our `GoogleProvider` is meant to be used with [https://developers.google.com/identity/protocols/oauth2](https://developers.google.com/identity/protocols/oauth2).

```cfscript
component {
  public any function configure(){
    return {
      "providers" : [
        {
          // name is optional, can be used to control the redirect uri
          // with name:     https://your.app.com/cbsso/auth/g
          // without name:  https://your.app.com/cbsso/auth/google
          name:         "g",
          type:         "GoogleProvider@cbsso",

          // these values are configured with Microsoft and available in your app dashboard
          clientId:     "YOUR-CLIENT-ID",
          clientSecret: "YOUR-CLIENT-SECRET",
          
          // optional - this is the default
          scope: "openid profile email"
        }
      ]
    };  
  }
}
```
