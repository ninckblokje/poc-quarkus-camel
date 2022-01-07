# poc-quarkus-camel-oidc

In this PoC Quarkus and Camel are using OpenID Connect with Azure AD to do authentication and authorization.

There are two authenticated endpoints available:
- /api/resteasy/hello
- /api/camel/hello

The first endpoint is implemented with JAX-RS (RESTEasy), the second endpoint is implemented using Camel REST DSL.

Note: Azure AD does not expose an introspection endpoint

## Security configuration

The JAX-RS endpoint is secured using the `@RolesAllowed` annotation.

The Camel endpoint is secured using properties in `application.properties`:

````
quarkus.http.auth.policy.greeting-read-policy.roles-allowed=Greeting.Read

quarkus.http.auth.permission.greating-read.paths=/api/camel/hello
quarkus.http.auth.permission.greating-read.policy=greeting-read-policy
quarkus.http.auth.permission.greating-read.methods=GET
````

The class `io.quarkus.security.identity.SecurityIdentity` can be used to get the principle. In a JAX-RS controller this class can be injected. In a Camel route an instance of this class can be found in the header `CamelVertxPlatformHttpAuthenticatedUser`.

## OpenID Connect configuration

The following properties are required:

````
#quarkus.oidc.client-id=
#quarkus.oidc.tenant-id=
#quarkus.oidc.credentials.secret=
#quarkus.oidc.roles.role-claim-path=roles   // location in Azure token where roles can be found
#quarkus.oidc.application-type=web_app
#quarkus.oidc.auth-server-url=
````

## Azure App Registration

Create an app registration is Azure AD with the following:
- Configured as web application with two redirect URL's:
  - http://localhost:8080/api/resteasy/hello
  - http://localhost:8080/api/camel/hello
- A client secret
- One API permission:
  - User.Read
- Exposed scope with admin and user consent:
  - User.Read
- One app role for applications, groups and users:
  - Greeting.Read

## Documentation

- https://quarkus.io/guides/security-openid-connect
- https://quarkus.io/guides/security-openid-connect-web-authentication
- https://quarkus.io/guides/security-authorization
