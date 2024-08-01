# How It Works

Even though there are multiple different SSO strategies out in the wild they generally tend to work on similiar principles. A user visits a website (the service provider) and is provided a set of options for authentication (identity providers). The user selects one of the presented identity providers and proceedes to login in and authorize the service provider to access some of their information. The identity provider then redirects the user back to the web application and confirms to the service provider that the user is who they say they are.

This module provides the utilities necessary to accomodate this pattern while being flexible enough to allow for the differences between different protocols and identity providers.&#x20;

When installed cbSSO provides some routes to your application.

`/cbsso/auth/:providerName/start`

This route is used by your application to determine which provider a user has selected and to provide the application a chance to do any work necessary before redirecting to the identity provider service.

`/cbsso/auth/:providerName`

This route is used as the redirect URI for the identity provider to communicate back to your application.
