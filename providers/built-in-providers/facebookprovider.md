# FacebookProvider

To use Facebook as an identity provider you will need to configure the `FacebookProvider` . In order to fully configure it you will need to register for a Facebook developer account on their developer site.

{% embed url="https://developers.facebook.com/" %}

```cfscript
component {
  public any function configure(){
    return {
      "providers" : [
        {
          // name is optional, can be used to control the redirect uri
          // with name:     https://your.app.com/cbsso/auth/fbook
          // without name:  https://your.app.com/cbsso/auth/Facebook
          name:         "fbook",
          type:         "FacebookProvider@cbsso",

          // these values are configured with Microsoft and available in your app dashboard
          clientId:     "YOUR-CLIENT-ID",
          clientSecret: "YOUR-CLIENT-SECRET",
          
          // optional - this is the default
          scope: "openid email"
        }
      ]
    };  
  }
}
```
