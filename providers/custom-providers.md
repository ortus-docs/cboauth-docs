# Custom Providers

In case you come across a situation where one of our provided providers doesn't provide for your prerequisites you can implement your own custom provider. Use the following template to get started.

<pre class="language-cfscript"><code class="lang-cfscript"><strong>// this can be anywhere but we will say it is in models/CustomProvider.cfc
</strong><strong>component implements="cbsso.models.ISSOIntegrationProvider" accessors=true {
</strong>    // custom properties you need
    property name="clientId";
    property name="clientSecret";

    // The name of your provider
    // this will be used to construct the redirect URI
    // in this case it will be /cbsso/auth/CustomProvider
    public string function getName(){
        return "CustomProvider";
    }
    
    // Return the icon url for easy rendering.
    // This can return an empty string if you don't use the
    // auto icon rendering feature of the ProviderService.
    public string function getIconURL(){
        return "";
    }

    // Return the auth endpoint URL of your identity provider.
    public string function startAuthenticationWorflow( required any event ){
        return "http://someidentityprovider.com/oauth/signin";
    }

    // This function will be called when the identity provider responds.
    // You should process the request from the IP and return a
    // properly configured instance of SSOAuthorizationResponse. 
    public any function processAuthorizationEvent( required any event ){
        var authResponse = wirebox.getInstance( "SSOAuthorizationResponse@cbsso" );

        return authResponse.setWasSuccessful( true )
            .setFirstName( event.getValue( "firstName" ) )
            .setEmail( event.getValue( "email" ) );
    }
}
</code></pre>

Once your `CustomProvider` is implemented you can configure it within your application like so.

```cfscript
component {
  public any function configure(){
    return {
      "providers" : [
        {
          // provide the proper dsl to target your custom provider
          type:         "CustomProvider",
          
          // any properties here will invoke their associated setter
          clientId:     "YOUR-CLIENT-ID",
          clientSecret: "YOUR-CLIENT-SECRET"
        }
      ]
    };  
  }
}
```
