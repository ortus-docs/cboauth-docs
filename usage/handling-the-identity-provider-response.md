# Handling The Identity Provider Response

While most identity providers follow similar protocols there is room for each one to provide slightly different workflows. Some providers use SAML, some use oAuth. And even if two providers use the same protocol there may be variation in how they use or present different pieces of information.

Part of cbSSO's goal is to help hide as much of these implementation details as possible. To accomplish this, we have created the `cbsso.models.ISSOAuthorizationResponse` interface and have provided an implementation as well.

After a SSO workflow has been initiated eventually the identity provider will respond to the initiating application. The format of the response varies by provider. To handle the responses each provider implements a method (`processAuthorizationEvent` )that will take the response, parse it, transform it into an  `ISSOAuthorizationResponse` and return it to your app for further processing.

### Handling The ISSOAuthorizationResponse

Once a response has been received and parsed cbSSO fires of the `CBSSOAuthorization` event. One way you could handle this event would be

```cfscript
public void function CBSSOAuthorization( event, data ){
    // the provider that was used for SSO
    var provider = data.provider;
    // an instance of ISSOAuthorizationResponse that contains our data
    var ssoAuthorizationResponse = data.ssoAuthorizationResponse;
    
    if( !ssoAuthorizationResponse.wasSuccessful() ){
        logger.error( "Failed SSO workflow: #ssoAuthorizationResponse.getErrorMessage()#" );
        relocate( "/login" );
    }
    
    var user = UserService.findByEmail( ssoAuthorizationResponse.getEmail() );
    
    // check if we have a user record for this person already
    if( isNull( user ) ){
        user = UserService.new();
        
        user.setEmail( ssoAuthorizationResponse.getEmail() );
        user.setFirstName( ssoAuthorizationResponse.getFirstName() );
        user.setLastName( ssoAuthorizationResponse.getLastName() );
        
        user.save();
    }
    
    // log them in - YAY SSO!
    user.login();
    
    relocate( "/dashboard" );
} 
```

### Behind The Scenes

Unless you implement a custom provider you shouldn't need to worry to much about how the SSO responses are handled. That being said, here is an example that shows how the GitHub provider implements this `processAuthorizationEvent( required any event )` method.

```cfscript
public any function processAuthorizationEvent( required any event ){
        // generate our SSOAuthorizationResponse which implements ISSOAuthorizationResponse
        var authResponse = wirebox.getInstance( "SSOAuthorizationResponse@cbsso" );
        var rawData = {
            "authResponse": {},
            "accessResponse": {},
            "userData": {}
        };
        authResponse.setRawResponseData( rawData );

        try {
            rawData[ "authResponse" ] = event.getCollection();

            // there was an error with our request or the user rejected us
            if( event.getValue( "error", "" ) != "" ){
                return authResponse
                    .setWasSuccessful( false )
                    .setErrorMessage( event.getValue( "error" ) );
            }

            // at this point we have been granted access on behalf of the user
            // so we can request an access_token
            var res = oAuthService.makeAccessTokenRequest(
                getClientId(),
                getClientSecret(),
                getRedirectUri(),
                getAccessTokenEndpoint(),
                event.getValue( "code" )
            );

            var accessData = parseAccessData( res.getData().toString() );
            rawData[ "accessResponse" ] = accessData;

            // request information about the current user
            var userDataRes = hyper
                .setMethod( "GET" )
                .setUrl( variables.userInfoURL )
                .setHeaders( { "Authorization": "Bearer " & accessData.access_token } )
                .send();

            // parse the user data
            var userData = deserializeJSON( userDataRes.getData() );
            rawData[ "userData" ] = userData;
            
            // transform our authorization data and user data into our interface format
            return authResponse
                .setWasSuccessful( true )
                .setName( userData.name )
                .setEmail( userData.email )
                .setUserId( userData.id )
        }
        catch( any e ){
            return authResponse
                .setWasSuccessful( false )
                .setErrorMessage( e.message );
        }        
    }
```
