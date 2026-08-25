# How It Works

Even though there are multiple different SSO strategies out in the wild they generally tend to work on similiar principles. A user visits a website (the service provider) and is provided a set of options for authentication (identity providers). The user selects one of the presented identity providers and proceedes to login in and authorize the service provider to access some of their information. The identity provider then redirects the user back to the web application and confirms to the service provider that the user is who they say they are.

This module provides the utilities necessary to accommodate this pattern while being flexible enough to allow for the differences between different protocols and identity providers.&#x20;

When installed cbSSO provides some routes to your application.

`/cbsso/auth/:providerName/start`

This route is used by your application to determine which provider a user has selected and to provide the application a chance to do any work necessary before redirecting to the identity provider service.

`/cbsso/auth/:providerName`

This route is used as the redirect URI for the identity provider to communicate back to your application.

## SAML request lifecycle

When the `MicrosoftSAMLProvider` starts a login, cbSSO generates an AuthNRequest with a unique request ID and
stores that ID in the configured CacheBox cache. The identity provider must return the same ID in both the
response and its bearer subject confirmation.

The response is accepted only after the signed assertion, issuer, audience, ACS recipient, request binding,
and validity window have been checked. Claims are then extracted from that verified assertion, and the request
ID is consumed from CacheBox. A second delivery of the same valid response is rejected.

The default cache is local to the application node. Configure `samlRequestCacheName` with a distributed
CacheBox cache when the application runs behind a load balancer; see [Configuration](configuration.md#saml-request-replay-cache).
