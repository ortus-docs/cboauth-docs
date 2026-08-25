# Configuration

An example `config/modules/cbsso.cfc` file.

```cfscript
component {
  public any function configure(){
    return {
      // enable this for cbAuth integration
      "enableCBAuthIntegration": false,
      
      // where cbSSO should redirect when an SSO has an unhandled error
      "errorRedirect": "",
      
      // where cbSSO should redirect after a successful process
      // you probably won't need this and will instead want to redirect manually
      // in the CBSSOOnAuthorization interception event
      "successRedirect": "",

      // CacheBox cache used for outstanding SAML authentication request IDs.
      // Leave blank to use the bounded cache created by cbSSO.
      "samlRequestCacheName": "",
      
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

### SAML request replay cache

The Microsoft SAML provider records each generated AuthNRequest ID and requires the same ID in the returned
SAML response and bearer subject confirmation. A successful response consumes the ID, so it cannot be used
again. This state is stored in CacheBox and does not depend on session management being enabled.

When `samlRequestCacheName` is blank, cbSSO creates a cache named `cbssoSAMLRequests` with a 10-minute
expiration, LRU eviction, a 10,000-entry limit, and an in-memory concurrent store. This default is suitable
for a single application node.

For a clustered deployment, configure a distributed CacheBox provider and select its cache by name:

```cfscript
// config/CacheBox.cfc
component {
  function configure(){
    cacheBox = {
      // Keep the defaultCache definition required by your application here.
      defaultCache: {
        objectDefaultTimeout: 120,
        maxObjects: 300
      },
      caches: {
        samlRequests: {
          provider: "path.to.YourDistributedCacheProvider",
          properties: {
            // Provider-specific Redis, database, or other shared-store settings.
          }
        }
      }
    };
  }
}

// config/modules/cbsso.cfc
component {
  function configure(){
    return {
      "samlRequestCacheName": "samlRequests"
    };
  }
}
```

The named cache must already be registered with CacheBox when cbSSO activates. If the configured cache does
not exist, module activation fails rather than silently falling back to local memory. The selected provider
must provide shared visibility and atomic removal when the application runs on multiple nodes.

### The enableCBAuthIntegration Setting

Provided out of the box but disabled by default is the ability to integrate your SSO workflow with cbAuth. In order to fully enable the integration between the two modules you will need to enable this setting and also implement some additional functions in your `UserService`.\
\
See [Enabling Integration](../cbauth-integration/enabling-integration.md) for more information.
