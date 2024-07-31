# Initiating SSO

There are two options for starting a SSO workflow. You can either present options to your user or initiate the flow programmatically.&#x20;

### Options

Ultimately providing SSO choices to your user will be an application specific task. That being said, many applications present their SSO integration in a similar fashion choosing to display some buttons which each initiate a SSO workflow with a different provider.&#x20;

### Displaying Choices to the User

This can be set up fairly easily using the `ProviderService`

```html
<!-- in views/login.cfm -->
<cfoutput>
    <cfscript>
        providerOptions = getInstance( "ProviderService@cbsso" ).getProviderOptions();
    </cfscript>
    <form action="/login">
        <h2>Login</h2>
        <input name="username" type="text" /> 
        <input name="password" type="password" /> 
        <button type="submit">Submit</button>
    </form>
    <p>-- or --</p>
    <div>
      <cfloop array="#providerOptions#" index="option" />
          <a class="link-button" href="#option.url#">Continue with #option.name#</a>
      </cfloop>  
    </div>
</cfoutput>
```

### Programmatically Starting SSO

To programmatically start a SSO workflow you can use the following code.

```cfscript
getInstance( "ProviderService@cbsso" ).start( providerName );
```

This will redirect the current request to the `/cbsso/auth/:providerName` endpoint of your application that is provided by this module.

### What Happens Next

Ultimately whether the user clicks a link or we redirect them programmatically they will end up at the same place. First, we will determine which of our configured providers they are using and then we will redirect them to the appropriate service.&#x20;

At this point the user is now the responsibility of the identity provider. If they have not logged in before they will be prompted to review the permissions scope of your application and decide whether or not they will grant permission to your application to see their user data. Whether they accept or reject the request they will be redirected back to your application and you will have a chance to handle the response.
