---
title: "Validation"
weight: 10
---

## IDynamicClientRegistrationValidator
The *IDynamicClientRegistrationValidator* is the contract for the service that
validates a dynamic client registration request. It contains a single
*ValidateAsync(...)* method.

Conceptually, the validation step is responsible for checking the validity of
the metadata supplied in the registration request, and using that metadata to
set properties of a *Client* model. In contrast, the
*IDynamicClientRegistrationRequestProcessor* is responsible for setting
properties on the *Client* model that are generated by the Configuration API
itself.

#### IDynamicClientRegistrationValidator.ValidateAsync

Validates a dynamic client registration request.

```csharp
public Task<IDynamicClientRegistrationValidationResult> ValidateAsync(
    DynamicClientRegistrationContext context)
```

| parameter | description |
| --- | --- |
| context | Contextual information about the DCR request. |

#### Return Value

A task that returns an [*IDynamicClientRegistrationValidationResult*]({{< ref "./models.md#idynamicclientregistrationvalidationresult" >}}), indicating success or failure.

## DynamicClientRegistrationValidator

```csharp
public class DynamicClientRegistrationValidator : IDynamicClientRegistrationValidator
```

The *DynamicClientRegistrationValidator* class is the default implementation of
the *IDynamicClientRegistrationValidator*. If you need to customize some aspect
of Dynamic Client Registration validation, we recommend that you extend this
class and override the appropriate methods.

## Validation Steps

Each of these methods represents one step in the validation process.
Each step is passed a [*DynamicClientRegistrationContext*]({{< ref
"./models.md#dynamicclientregistrationcontext" >}}) and returns a task
that returns an [*IStepResult*]({{< ref "./models.md#istepresult"
>}}). The *DynamicClientRegistrationContext* includes the client model that will
have its properties set, the DCR request, and other contextual information. The
*IStepResult* either represents that the step succeeded or failed.

The steps are invoked in the same order as they appear in this table.

| name | description |
| --- | --- |
| ValidateSoftwareStatementAsync(…) | Validates the software statement of the request. The default implementation does nothing, and is included as an extension point. |
| SetGrantTypesAsync(…) | Validates requested grant types and uses them to set the allowed grant types of the client. |
| SetRedirectUrisAsync(…) | Validates requested redirect uris and uses them to set the redirect uris of the client. |
| SetScopesAsync(…) | Validates requested scopes and uses them to set the scopes of the client. |
| SetDefaultScopes(…) | Sets scopes on the client when no scopes are requested. The default implementation sets no scopes and is intended as an extension point. |
| SetSecretsAsync(…) | Validates the requested jwks to set the secrets of the client. |
| SetClientNameAsync(…) | Validates the requested client name uses it to set the name of the client. |
| SetLogoutParametersAsync(…) | Validates the requested client parameters related to logout and uses them to set the corresponding properties in the client. Those parameters include the post logout redirect uris, front channel and back channel uris, and flags for the front and back channel uris indicating if they require session ids. |
| SetMaxAgeAsync(…) | Validates the requested default max age and uses it to set the user sso lifetime of the client. |
| SetUserInterfaceProperties(…) | Validates details of the request that control the user interface, including the logo uri, client uri, initiate login uri, enable local login flag, and identity provider restrictions, and uses them to set the corresponding client properties. |
| SetPublicClientProperties(…) | Validates the requested client parameters related to public clients and uses them to set the corresponding properties in the client. Those parameters include the require client secret flag and the allowed cors origins. |
| SetAccessTokenProperties(…) | Validates the requested client parameters related to access tokens and uses them to set the corresponding properties in the client. Those parameters include the allowed access token type and access token lifetime. |
| SetIdTokenProperties(…) | Validates the requested client parameters related to id tokens and uses them to set the corresponding properties in the client. Those parameters include the id token lifetime and the allowed id token signing algorithms. |
| SetServerSideSessionProperties(…) | Validates the requested client parameters related to server side sessions and uses them to set the corresponding properties in the client. Those parameters include the coordinate lifetime with user session flag. |
