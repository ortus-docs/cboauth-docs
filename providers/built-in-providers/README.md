# Built-in Providers

Out of the box we have implemented the following providers for your use.

* [FacebookProvider](https://app.gitbook.com/o/-LA-UVKtPMvMR4t5JRTB/s/bhpL1furbue2wKeid2C7/\~/changes/3/providers/built-in-providers/facebookprovider)
* [GitHubProvider](https://app.gitbook.com/o/-LA-UVKtPMvMR4t5JRTB/s/bhpL1furbue2wKeid2C7/\~/changes/3/providers/built-in-providers/githubprovider)
* [GoogleProvider](https://app.gitbook.com/o/-LA-UVKtPMvMR4t5JRTB/s/bhpL1furbue2wKeid2C7/\~/changes/3/providers/built-in-providers/githubprovider)
* [MicrosoftSAMLProvider](https://app.gitbook.com/o/-LA-UVKtPMvMR4t5JRTB/s/bhpL1furbue2wKeid2C7/\~/changes/3/providers/built-in-providers/mircosoftsamlprovider)

Each provider implements the `cbsso.models.ISSOIntegrationProvider` interface and provides the necessary functionality to interact with a specific identity provider.

This interface is primarily for use by the `ProviderService@cbsso` and is not necessarily intended for direct use but is available if you need it.

```cfscript
interface {
    public string function getName();
    public string function startAuthenticationWorflow( required any event );
    public any function processAuthorizationEvent( required any event );
}
```



