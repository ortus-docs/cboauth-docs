# Enabling Integration

CBSSO is buitl to be cbAuth aware out of the box. Setting up cbAuth is very similiar to writing your own interceptor for a custom implementation but reduces the boilerplate and is much more organized.

### Module Settings

Wherever you have configured your cbSSO settings you will need to set `enableCBAuthIntegration` to `true`.&#x20;

By enabling this setting cbSSO will register an interceptor that provides the basic pattern for receiving a response from an SSO provider and connecting it to a user. While cbSSO is able to enforce a general pattern for handling the SSO authentication flow it needs the application to provide some specific functionality. It does this through interacting with your application's implementation of cbAuth's `IUserService`.

### Necessary IUserService Additions

Integrating cbSSO and cbAuth requires that your `IUserService` implement the following functions.

```cfscript
/**
     * This function is used to tell cbSSO which user is associated with an ssoAuthorizationResponse. 
     * 
     * @param ssoAuthorizationResponse An instance of ISSOAuthorizationResponse that was successful
     * @param provider The configured provider used for this SSO event
     *
     * @return An cbAuth.models.IUser instance or null
     */
    public any function findBySSO( required any ssoAuthorizationResponse, required any provider );

    /**
     * Create a new user based off of information from the ISSOAuthorizationResponse.
     * 
     * @param ssoAuthorizationResponse An instance of ISSOAuthorizationResponse that was successful
     * @param provider The configured provider used for this SSO event
     *
     * @return An cbAuth.models.IUser instance
     */
    public any function createFromSSO( required any ssoAuthorizationResponse, required any provider );

    /**
     * Update an existing user based off of the ssoAuthorizationResponse.
     * 
     * @param The user associated with the ssoAuthorizationResponse
     * @param ssoAuthorizationResponse An instance of ISSOAuthorizationResponse that was successful
     * @param provider The configured provider used for this SSO event
     *
     */
    public void function updateFromSSO( required any user, required any ssoAuthorizationResponse, required any provider );
```

### Running It

Once these pieces are all in place you should have a fully working SSO implementation fully integrated with cbAuth!
