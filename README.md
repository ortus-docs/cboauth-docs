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

## Versioning <a href="#versioning" id="versioning"></a>

The cbSSO module is maintained under the [Semantic Versioning](http://semver.org/) guidelines as much as possible. Releases will be numbered in the following format:

```
<major>.<minor>.<patch>
```

And constructed with the following guidelines:

* Breaking backward compatibility bumps the major (and resets the minor and patch)
* New additions without breaking backward compatibility bumps the minor (and resets the patch)
* Bug fixes and misc changes bumps the patch

## License <a href="#license" id="license"></a>

Apache 2 License: [http://www.apache.org/licenses/LICENSE-2.0](https://www.apache.org/licenses/LICENSE-2.0)​

## Important Links <a href="#important-links" id="important-links"></a>

* Code: [https://github.com/coldbox-modules/cbsso](https://github.com/coldbox-modules/cbsso)
* Issues: [https://github.com/coldbox-modules/cbsso/issues](https://github.com/coldbox-modules/cbsso/issues)

## Professional Open Source <a href="#professional-open-source" id="professional-open-source"></a>

![Ortus Solutions, Corp](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LA-UVvG0NM7NpDzssBL%2F-LA-Uaei0WzTH7Su5CR7%2F-LA-UqN1BRXynZ7RUVO7%2Fortussolutions\_button.png?generation=1523647999385555\&alt=media)

The cbSSO module is a professional open-source software backed by [Ortus Solutions, Corp](http://www.ortussolutions.com/services) offering services like:

* Custom Development
* Professional Support & Mentoring
* Training
* Server Tuning
* Security Hardening
* Code Reviews
* [Much More](http://www.ortussolutions.com/services)

### HONOR GOES TO GOD ABOVE ALL <a href="#honor-goes-to-god-above-all" id="honor-goes-to-god-above-all"></a>

Because of His grace, this project exists. If you don't like this, then don't read it; it's not for you.

> "Therefore being justified by **faith**, we have peace with God through our Lord Jesus Christ: By whom also we have access by **faith** into this **grace** wherein we stand, and rejoice in hope of the glory of God." Romans 5:5

