---
description: >-
  Welcome to cbSSO a ColdBox module to help integrate SSO into your application
  easily.
---

# Introduction

Bundled in this module are several SSO provider implementations that allow you to quickly and easily integrate with Identity Providers such as Microsoft, Google, GitHub or Facebook using standard protocols like SAML and oAuth.

To install run

`box install cbsso`

Once installed you can configure your settings like so

```cfscript
// config/modules/cbsso.cfc
component {
    function configure(){
        return {
            "providers": [
                {
                    type: "GoogleProvider@cbsso",
                    clientId: getJavaSystem().getProperty( "GOOGLE_CLIENT_ID" ),
                    clientSecret: getJavaSystem().getProperty( "GOOGLE_CLIENT_SECRET" )
                }
            ]
        };
    }
}
```

Your app now has the ability to direct users to Google for authentication!

The rest of this documentation will cover topics like

* Built-in Providers
* Custom Providers
* Configuration
* Accessing User Data



