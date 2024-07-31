# GitHubProvider

The `GitHubProvider` can be configured to interact with Github. You can find their documentation here [https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/authorizing-oauth-apps#web-application-flow](https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/authorizing-oauth-apps#web-application-flow)

```cfscript
component {
  public any function configure(){
    return {
      "providers" : [
        {
          // name is optional, can be used to control the redirect uri
          // with name:     https://your.app.com/cbsso/auth/g
          // without name:  https://your.app.com/cbsso/auth/GitHub
          name:         "g",
          type:         "GitHubProvider@cbsso",

          // these values are configured with Microsoft and available in your app dashboard
          clientId:     "YOUR-CLIENT-ID",
          clientSecret: "YOUR-CLIENT-SECRET",
          
          // optional - this is the default
          scope: "user user:email"
        }
      ]
    };  
  }
}
```
