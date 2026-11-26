# OAuth 2.0, OpenID Connect (OIDC) and JWT 

# OAuth 2.0 – Client, Server, Client ID, Client Secret, Scopes and Token Flow

## 1. Introduction

OAuth 2.0 is an authorization framework that allows applications to access protected resources without sharing user credentials with every application.

It defines how an application obtains an access token and uses that token to access a protected API.

Example:

- Frontend: React Application
- Identity Provider: Okta
- Backend: Spring Boot REST API
- Database:  DB

## 2. Main Components

OAuth 2.0 involves three important components.

### 2.1 OAuth Client

The client is the application requesting access to a protected resource.

Examples:
- React application
- Mobile application
- Spring Boot microservice
- Server-side web application

Responsibilities:
- Initiates the authorization process.
- Requests required scopes.
- Obtains access tokens.
- Sends access tokens to protected APIs.

### 2.2 Authorization Server

The Authorization Server authenticates the client and issues access tokens.

Examples:
- Okta
- Keycloak
- Microsoft Entra ID
- Amazon Cognito

Responsibilities:
- Authenticates users when applicable.
- Registers and manages client applications.
- Validates client credentials.
- Defines and evaluates scopes and permissions.
- Issues access tokens.
- Issues refresh tokens when supported.
- Publishes public keys for JWT verification.

### 2.3 Resource Server

The Resource Server hosts protected APIs and validates access tokens.

Example:
- Spring Boot REST API

Responsibilities:
- Receives access tokens.
- Validates JWT signatures.
- Validates issuer and expiration.
- Validates audience when configured.
- Checks scopes and authorities.
- Allows or denies API requests.

## 3. Client vs Authorization Server vs Resource Server

| Component | Example | Responsibility |
|---|---|---|
| OAuth Client | React Application | Requests access |
| Authorization Server | Okta | Issues tokens |
| Resource Server | Spring Boot API | Protects resources |

Architecture:

```text
             User
               |
               v
       React Application
          OAuth Client
               |
               | Authorization Request
               v
        Okta Authorization Server
               |
               | Access Token
               v
       React Application
               |
               | Bearer Access Token
               v
       Spring Boot REST API
          Resource Server
               |
               | Validated Request
               v
            Database
```

## 4. Client Registration

Before an application can use OAuth, it must be registered with the Authorization Server.

Example using Okta:

1. Log in to the Okta Admin Console.
2. Navigate to Applications.
3. Create a new application integration.
4. Select the appropriate OIDC application type.
5. Configure redirect URIs.
6. Configure allowed grant types.
7. Configure required scopes and permissions.
8. Register the application.
9. Obtain the Client ID.
10. Obtain a Client Secret if the application is a confidential client.

The application registration establishes its identity and permitted authentication configuration.

## 5. Client ID

A Client ID uniquely identifies an application registered with the Authorization Server.

Example:

```text
Client ID: mylifepath-web
```

Characteristics:

- Generated during application registration.
- Identifies the application.
- Not considered confidential.
- Included in authorization requests.
- Used by the Authorization Server to identify the registered client.

Think of Client ID as the application's username.

## 6. Client Secret

A Client Secret is a confidential credential used by a confidential OAuth client to authenticate itself to the Authorization Server.

Example:

```text
Client ID: mylifepath-backend
Client Secret: abc123-secret-value
```

Characteristics:

- Generated for confidential clients.
- Used when authenticating the client.
- Must be protected.
- Must never be exposed in frontend JavaScript.
- Should be stored in a secrets manager.

Examples of secret management systems:

- AWS Secrets Manager
- Azure Key Vault
- HashiCorp Vault

Think of Client Secret as the application's password.

Never commit Client Secrets to GitHub.

## 7. Public Client vs Confidential Client

### Public Client

A public client cannot securely protect a Client Secret.

Examples:
- React SPA
- Mobile application
- Desktop application

Characteristics:
- Has a Client ID.
- Does not use a Client Secret for authentication.
- Uses Authorization Code Flow with PKCE.
- Runs in an environment where users can inspect application code.

### Confidential Client

A confidential client can securely protect its credentials.

Examples:
- Server-side Spring Boot application
- Backend microservice
- Server-side web application

Characteristics:
- Has a Client ID.
- Can have a Client Secret.
- Can authenticate itself to the Authorization Server.
- Can use Client Credentials Flow for service-to-service communication.

| Feature | Public Client | Confidential Client |
|---|---|---|
| Client ID | Yes | Yes |
| Client Secret | No | Yes, typically |
| Secure credential storage | Cannot protect a shared secret | Can protect credentials |
| PKCE | Used for authorization code flow | Also supported and recommended where applicable |
| Example | React SPA | Spring Boot backend |

## 8. OAuth Scopes

A scope represents a permission that a client requests to access a protected resource.

Example:

```text
accounts:read
accounts:write
transactions:read
admin:read
```

Example scope definitions:

| Scope | Description |
|---|---|
| accounts:read | Read account information |
| accounts:write | Update account information |
| transactions:read | Read transaction details |
| admin:read | Access administrative information |

### Who Creates Scopes?

The API owner or security administrator defines scopes in the Authorization Server configuration.

For example, an administrator configures custom scopes in Okta.

The client requests the scopes it needs.

The Authorization Server evaluates whether the client or user is allowed to receive those scopes.

Important:

Requesting a scope does not automatically grant permission.

The Authorization Server must authorize it, and the Resource Server must enforce it.

## 9. Roles vs Scopes

Roles and scopes are related but have different purposes.

### Roles

Roles represent a user's or application's responsibility.

Examples:
- CUSTOMER
- ADMIN
- OPERATIONS
- AUDITOR

### Scopes

Scopes represent permissions to access API resources.

Examples:
- accounts:read
- accounts:write
- transactions:read

Example:

| Role | Scopes |
|---|---|
| CUSTOMER | accounts:read, transactions:read |
| ADMIN | accounts:read, accounts:write, admin:read |
| AUDITOR | accounts:read, transactions:read |

The application can enforce authorization using roles, scopes, or both.

## 10. Redirect URI

A Redirect URI is the location where the Authorization Server redirects the user after authentication.

Example:

```text
https://mylifepath.example.com/callback
```

The application registers this URI with the Authorization Server.

During authorization, the client sends the redirect URI.

The Authorization Server validates it against the registered URI.

This prevents an attacker from redirecting authorization responses to an untrusted location.

## 11. Authorization Endpoint

The Authorization Endpoint is where the client sends the user to authenticate and authorize access.

Example:

```text
https://your-okta-domain/oauth2/default/v1/authorize
```

It is used in authorization flows such as Authorization Code Flow.

Typical parameters:

```text
client_id
response_type
redirect_uri
scope
state
code_challenge
code_challenge_method
```

## 12. Token Endpoint

The Token Endpoint is where the client exchanges an authorization code or client credentials for tokens.

Example:

```text
https://your-okta-domain/oauth2/default/v1/token
```

Common parameters depend on the grant type.

Authorization Code:

```text
grant_type=authorization_code
code=AUTHORIZATION_CODE
redirect_uri=REGISTERED_REDIRECT_URI
client_id=CLIENT_ID
code_verifier=PKCE_CODE_VERIFIER
```

Client Credentials:

```text
grant_type=client_credentials
scope=accounts:read
```

For a confidential client, client authentication is also required.

## 13. Issuer vs Audience

These are important JWT claims.

### Issuer (iss)

Identifies who issued the token.

Example:

```json
{
  "iss": "https://your-okta-domain/oauth2/default"
}
```

### Audience (aud)

Identifies the intended recipient of the token.

Example:

```json
{
  "aud": "mylifepath-api"
}
```

The Resource Server should validate that the token was issued by the trusted issuer and intended for that API.

## 14. JWT Claims

JWT contains information called claims.

Example payload:

```json
{
  "sub": "user123",
  "iss": "https://your-okta-domain/oauth2/default",
  "aud": "mylifepath-api",
  "scope": "accounts:read transactions:read",
  "exp": 1795700000,
  "iat": 1795696400
}
```

Common claims:

| Claim | Description |
|---|---|
| sub | Subject or user/service identity |
| iss | Token issuer |
| aud | Intended audience |
| exp | Token expiration |
| iat | Token issue time |
| nbf | Token validity start time |
| scope | Granted permissions |
| jti | Unique token identifier |

The actual claims depend on the Identity Provider and token configuration.

## 15. State Parameter

State is a random value generated by the client before starting an authorization request.

The client sends it to the Authorization Server.

The Authorization Server returns the same state value in the redirect response.

The client verifies that the returned value matches the original value.

Purpose:

- Protects against Cross-Site Request Forgery (CSRF).
- Correlates authorization requests and responses.

## 16. Nonce Parameter

Nonce is a random value used in OpenID Connect authentication requests.

The Authorization Server includes the nonce in the ID Token.

The client verifies the returned nonce.

Purpose:

- Helps protect against replay attacks.
- Associates the ID Token with the original authentication request.

## 17. PKCE

PKCE stands for Proof Key for Code Exchange.

It protects Authorization Code Flow, especially for public clients such as React and mobile applications.

Two important values:

- Code Verifier: A random secret generated by the client.
- Code Challenge: A derived value sent in the authorization request.

Flow:

1. Client generates a random code verifier.
2. Client calculates the code challenge.
3. Client sends the code challenge to the Authorization Server.
4. Authorization Server returns an authorization code.
5. Client sends the authorization code and original code verifier to the Token Endpoint.
6. Authorization Server verifies the code verifier against the original challenge.
7. Authorization Server issues tokens if validation succeeds.

PKCE prevents an attacker who intercepts an authorization code from redeeming it without the original verifier.

## 18. Authorization Code Flow with PKCE

Example:

- Client: React Application
- Authorization Server: Okta
- Resource Server: Spring Boot API

Steps:

1. User opens the React application.
2. React generates state, nonce, and PKCE values.
3. React redirects the user to Okta's Authorization Endpoint.
4. Okta authenticates the user.
5. Okta redirects the browser to the registered Redirect URI with an authorization code and state.
6. React validates state.
7. React sends the authorization code and PKCE verifier to the Token Endpoint.
8. Okta validates the code and verifier.
9. Okta issues an Access Token and ID Token.
10. React sends the Access Token to Spring Boot.
11. Spring Security validates the token.
12. Spring Boot authorizes and processes the request.

## 19. Client Credentials Flow

Client Credentials Flow is used for machine-to-machine communication.

Example:

- Payment Service: OAuth Client
- Account Service: Resource Server
- Okta: Authorization Server

There is no end-user login.

Steps:

1. Payment Service authenticates itself using its Client ID and Client Secret.
2. Payment Service requests an access token.
3. It requests the required scope, such as accounts:read.
4. Okta validates the client credentials and permissions.
5. Okta issues an access token.
6. Payment Service calls Account Service using the access token.
7. Account Service validates the token and checks permissions.
8. Account Service returns the requested data.

Example token request:

```http
POST /oauth2/default/v1/token
Content-Type: application/x-www-form-urlencoded
Authorization: Basic BASE64_CLIENT_CREDENTIALS

grant_type=client_credentials&scope=accounts:read
```

The token endpoint's client-authentication method must match the registered client configuration.

## 20. Access Token vs ID Token vs Refresh Token

| Token | Purpose | Used By |
|---|---|---|
| Access Token | Access protected APIs | OAuth Client |
| ID Token | Communicate authenticated user identity | OIDC Client |
| Refresh Token | Obtain a replacement access token | OAuth Client |

### Access Token

Used to call protected APIs.

Example:

```http
Authorization: Bearer ACCESS_TOKEN
```

### ID Token

Contains authentication information about the user.

It is intended for the OIDC client, not as the API's access credential.

### Refresh Token

Allows the client to obtain a new access token without asking the user to authenticate again, subject to the provider's policies.

Refresh tokens must be protected like other sensitive credentials.

## 21. JWKS – JSON Web Key Set

JWKS contains public keys published by the Authorization Server.

The Resource Server uses these public keys to verify JWT signatures.

Example:

```text
https://your-okta-domain/oauth2/default/v1/keys
```

Typical validation process:

1. Spring Security receives a JWT.
2. It identifies the signing key using the JWT key ID (kid).
3. It obtains the corresponding public key from JWKS.
4. It verifies the JWT signature.
5. It validates configured claims such as issuer and expiration.
6. It validates audience and other required claims when configured.

The Resource Server does not need the Authorization Server's private signing key.

## 22. Token Introspection

Token introspection is an endpoint used to determine whether a token is active and retrieve associated token information.

It is commonly used with opaque tokens.

Example response:

```json
{
  "active": true,
  "scope": "accounts:read",
  "client_id": "payment-service",
  "sub": "payment-service"
}
```

JWT Resource Servers commonly validate JWTs locally using signatures and claims instead of making an introspection request for every API call.

## 23. Token Revocation

Token revocation allows a client to request that a token be invalidated.

Example situations:

- User logs out.
- Application credentials are compromised.
- Access needs to be withdrawn.
- A refresh token must be invalidated.

The actual effect depends on the token type and Authorization Server implementation.

A locally validated JWT may remain usable until expiration unless additional revocation or denylist mechanisms are implemented.

## 24. OAuth Error Responses

| Error | Meaning |
|---|---|
| invalid_request | Missing or invalid request parameter |
| invalid_client | Client authentication failed |
| invalid_grant | Invalid or expired authorization grant |
| unauthorized_client | Client is not authorized for the grant type |
| unsupported_grant_type | Unsupported OAuth grant |
| invalid_scope | Requested scope is invalid or unavailable |
| access_denied | Authorization was denied |

HTTP status codes commonly returned:

- 400 Bad Request
- 401 Unauthorized
- 403 Forbidden

## 25. OAuth in Spring Boot

Spring Boot can act as an OAuth2 Resource Server.

### Maven Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-resource-server</artifactId>
</dependency>
```

### application.yml

```yaml
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: https://your-okta-domain/oauth2/default
```

### Security Configuration

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    SecurityFilterChain securityFilterChain(
            HttpSecurity http) throws Exception {

        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/accounts/**")
                    .hasAuthority("SCOPE_accounts:read")
                .anyRequest().authenticated()
            )
            .oauth2ResourceServer(oauth ->
                oauth.jwt(Customizer.withDefaults())
            );

        return http.build();
    }
}
```

This configuration:

1. Configures Spring Security as a Resource Server.
2. Extracts bearer tokens from incoming requests.
3. Validates JWT signatures and configured claims.
4. Converts recognized scopes into authorities.
5. Checks the required scope.
6. Allows or rejects API access.

Audience validation should be configured explicitly if the API requires a particular audience.

## 26. OAuth Security Best Practices

1. Use HTTPS for all OAuth communication.
2. Never expose Client Secrets in frontend code.
3. Store secrets in a secrets manager.
4. Use Authorization Code Flow with PKCE for browser and mobile public clients.
5. Use Client Credentials Flow for machine-to-machine access.
6. Request only the scopes required by the application.
7. Validate JWT signatures.
8. Validate issuer and expiration.
9. Validate audience for the intended API.
10. Use short-lived access tokens where appropriate.
11. Protect refresh tokens.
12. Rotate client credentials.
13. Validate Redirect URIs strictly.
14. Use state to protect authorization redirects.
15. Use nonce for OIDC authentication.
16. Apply role- and scope-based authorization.
17. Avoid logging tokens or secrets.
18. Implement tenant-level authorization where applicable.

## 27. Quick Reference

| Question | Answer |
|---|---|
| Who creates Client ID? | Identity Provider during application registration |
| Who creates Client Secret? | Identity Provider for confidential clients |
| Who defines scopes? | API owner/security administrator |
| Who requests scopes? | OAuth Client |
| Who issues Access Tokens? | Authorization Server |
| Who validates Access Tokens? | Resource Server |
| Who authenticates users? | Identity Provider |
| What is Redirect URI? | Client callback location |
| What is Issuer? | Token issuing authority |
| What is Audience? | Intended token recipient |
| What is PKCE? | Authorization-code protection mechanism |
| What is JWKS? | Set of public keys used for JWT verification |
| What is State? | CSRF protection and request correlation |
| What is Nonce? | OIDC replay protection and request correlation |
| What is Client Credentials? | Machine-to-machine authorization flow |
| What is Scope? | Permission requested for API access |

## 28. Official References

- OAuth 2.0: https://www.rfc-editor.org/rfc/rfc6749
- OAuth 2.0 Security Best Current Practice: https://www.rfc-editor.org/rfc/rfc9700
- OpenID Connect Core: https://openid.net/specs/openid-connect-core-1_0.html
- OAuth 2.0 PKCE: https://www.rfc-editor.org/rfc/rfc7636
- Okta Developer Documentation: https://developer.okta.com/docs/guides/
- Spring Security Resource Server: https://docs.spring.io/spring-security/reference/servlet/oauth2/resource-server/jwt.html


## 1. What is OAuth 2.0?

**Answer:**

OAuth 2.0 is an authorization framework that allows an application to access a user's resources without directly collecting or storing the user's password.

It works using access tokens.

For example:
- A user logs into an application.
- The application redirects the user to an authorization server.
- The user authenticates and grants permission.
- The authorization server issues an access token.
- The application uses that token to access protected APIs.

**Real-world example:** Login to an application using Google and allow it to access your Google profile.

Important: OAuth 2.0 primarily handles authorization, not authentication.

## 2. What is the difference between authentication and authorization?

**Authentication:** Verifies who you are.

Example: Logging into an application using your username and password.

**Authorization:** Determines what you are allowed to access.

Example: An authenticated user can view their account but cannot access an administrator's account.

Remember:

- Authentication = Who are you?
- Authorization = What can you access?

## 3. What are the main components of OAuth 2.0?

There are four important roles:

1. Resource Owner: The user who owns the data.
2. Client: The application requesting access to the user's resources.
3. Authorization Server: Authenticates the user and issues access tokens.
4. Resource Server: Hosts protected APIs and validates access tokens.

Example:



## 4. What is an access token?

An access token is a credential that allows an application to access protected resources.

The client sends the token in the HTTP Authorization header.

Example:

```http
GET /api/accounts
Authorization: Bearer <access_token>
```

The resource server validates the token before allowing access.

Access tokens generally have a limited lifetime.

## 5. What is a refresh token?

A refresh token is used to obtain a new access token without requiring the user to authenticate again.

Typical flow:

1. User authenticates.
2. Authorization server issues an access token and, when applicable, a refresh token.
3. Access token expires.
4. Client uses the refresh token to request a new access token.
5. Authorization server validates the refresh token and issues a new access token.

Refresh tokens must be protected carefully because they can be used to obtain new access tokens.

## 6. What is the difference between OAuth 2.0 and OpenID Connect?

**OAuth 2.0:**
- Authorization framework.
- Provides access tokens.
- Allows applications to access protected resources.

**OpenID Connect (OIDC):**
- Authentication layer built on top of OAuth 2.0.
- Introduces an ID token.
- Provides information about the authenticated user.

Example:

When a user logs in using Okta:

- OIDC authenticates the user.
- The ID token provides identity information.
- The access token allows access to protected APIs.

Remember:

OAuth 2.0 = Authorization.

OIDC = Authentication built on OAuth 2.0.

## 7. What is JWT?

JWT stands for JSON Web Token.

It is a compact token format commonly used to securely transmit claims between parties.

A JWT has three parts:

1. Header
2. Payload
3. Signature

Example:

```text
xxxxx.yyyyy.zzzzz
```

**Header:**
Contains token type and signing algorithm.

**Payload:**
Contains claims such as:
- sub: Subject or user identifier.
- iss: Issuer.
- aud: Audience.
- exp: Expiration time.
- scope: Permissions.

**Signature:**
Used to verify that the token has not been modified.

Important: A signed JWT is not necessarily encrypted. Do not put passwords or sensitive information in its payload.

## 8. How does JWT validation work?

When a client sends a JWT to a backend API:

1. Backend extracts the bearer token.
2. Validates the token signature.
3. Verifies the issuer (iss).
4. Verifies the audience (aud).
5. Checks the expiration time (exp).
6. Checks relevant permissions or scopes.
7. Allows or denies access.

If validation fails, the API rejects the request.

Typically:
- HTTP 401: Missing or invalid authentication.
- HTTP 403: Authenticated but insufficient permissions.

## 9. What is the difference between JWT and OAuth 2.0?

They are not competing technologies.

OAuth 2.0 is an authorization framework.

JWT is a token format.

OAuth 2.0 access tokens can be JWTs, but they can also be opaque tokens.

JWT describes how token information is represented, while OAuth 2.0 describes how authorization is obtained and used.

## 10. What are OAuth 2.0 grant types?

Important flows:

### A. Authorization Code Flow with PKCE

Recommended for modern user-facing applications, including public clients.

Steps:

1. User opens the application.
2. Application redirects the user to the authorization server.
3. User authenticates.
4. Authorization server redirects back with an authorization code.
5. Client exchanges the code and PKCE verifier for tokens.
6. Client accesses protected APIs using the access token.

PKCE protects the authorization code exchange against interception.

### B. Client Credentials Flow

Used for machine-to-machine communication.

Example:

One backend microservice needs to access another backend microservice.

Steps:

1. Client sends its client ID and client secret to the authorization server.
2. Authorization server validates the client.
3. Authorization server issues an access token.
4. Client sends the token to the resource server.
5. Resource server validates the token and grants access according to permissions.

There is no end-user login in this flow.

### C. Refresh Token Flow

Used to obtain a new access token using a valid refresh token.

### D. Implicit Flow

An older OAuth flow that returned access tokens directly through the browser.

It is deprecated for modern OAuth implementations. Authorization Code with PKCE is preferred.

### E. Resource Owner Password Credentials Flow

An older flow where the client directly collected the user's password.

It is deprecated and should not be used in new implementations.

## 11. What is PKCE?

PKCE stands for Proof Key for Code Exchange.

It adds protection to the OAuth Authorization Code Flow.

It helps prevent attackers who intercept an authorization code from exchanging it for tokens.

Two important values:

- Code Verifier: A randomly generated secret.
- Code Challenge: A transformed version of the verifier, typically using SHA-256.

The authorization server verifies the code verifier during token exchange.

## 12. What are OAuth scopes?

Scopes define the permissions requested by a client.

Examples:

```text
read:accounts
write:accounts
read:transactions
```

If an application has only `read:accounts`, it should not be allowed to modify accounts.

Scopes support least-privilege access.

## 13. What is the difference between access token and ID token?

| Access Token | ID Token |
|---|---|
| Used for API authorization | Used for user authentication |
| Presented to resource servers | Intended for the OIDC client |
| Represents granted access | Contains identity claims |
| Can be JWT or opaque | JWT format in OIDC |

Do not use an ID token as a substitute for an API access token.

## 14. What is the difference between symmetric and asymmetric signing?

**Symmetric signing:**
- Uses the same secret key for signing and verification.
- Example: HS256.
- Both parties need access to the shared secret.

**Asymmetric signing:**
- Uses a private key to sign.
- Uses a public key to verify.
- Example: RS256.
- Public keys can be distributed to multiple resource servers without sharing the private signing key.

For enterprise distributed systems, asymmetric signing is commonly used.

## 15. What is JWKS?

JWKS stands for JSON Web Key Set.

It is a set of public keys published by an authorization server.

Resource servers use these keys to verify JWT signatures.

Typical process:

1. API receives a JWT.
2. Reads the `kid` from the JWT header.
3. Retrieves the matching public key from the JWKS endpoint.
4. Verifies the signature.
5. Validates token claims.

Public keys are typically cached and refreshed when necessary, including when key rotation occurs.

## 16. What is the role of an API Gateway in OAuth?

An API Gateway acts as the entry point for incoming API requests.

It can handle:

- Authentication and token validation.
- Authorization and access policies.
- Rate limiting.
- Request routing.
- TLS termination.
- Logging and monitoring.

Example architecture:

```text
User
  |
  v
Frontend Application
  |
  v
Identity Provider (Okta)
  |
  v
Access Token
  |
  v
API Gateway
  |
  v
Backend Microservices
  |
  v
Database
```

The gateway can validate the token, but backend services should also enforce authorization for their own sensitive resources where appropriate.

## 17. How do you implement OAuth security in Spring Boot?

Spring Security provides OAuth 2.0 and OIDC support.

For a Spring Boot resource server:

1. Configure Spring Security.
2. Configure the issuer URI.
3. Enable JWT resource server support.
4. Spring Security validates incoming bearer tokens.
5. Configure authorization rules based on scopes or roles.
6. Protect individual endpoints.

Example configuration:

```yaml
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: https://your-idp.example.com
```

Example Java configuration:

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    SecurityFilterChain securityFilterChain(
            HttpSecurity http) throws Exception {

        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/public/**").permitAll()
                .requestMatchers("/admin/**")
                    .hasAuthority("SCOPE_admin")
                .anyRequest().authenticated()
            )
            .oauth2ResourceServer(oauth2 ->
                oauth2.jwt(Customizer.withDefaults())
            );

        return http.build();
    }
}
```

This example configures a JWT resource server. Production implementations should also configure appropriate CORS, CSRF, session, and token-validation policies for their application architecture.

## 18. What happens if an access token expires?

When an access token expires:

1. The backend rejects the request.
2. The client may use its refresh token to obtain a new access token.
3. The client retries the request using the new access token.
4. If refresh fails, the user may need to authenticate again.

A resource server should not accept expired tokens.

## 19. How do you secure microservices using OAuth?

For enterprise microservices:

1. Use a centralized identity provider.
2. Use OAuth 2.0 access tokens.
3. Validate tokens at the API gateway and relevant backend services.
4. Use client credentials for machine-to-machine authentication.
5. Apply scopes and roles for authorization.
6. Use HTTPS for communication.
7. Apply least-privilege permissions.
8. Rotate secrets and signing keys.
9. Monitor authentication failures and suspicious access.

For sensitive service-to-service communication, mTLS can provide an additional layer of authentication.

## 20. What is the difference between OAuth 2.0 and SAML?

| OAuth 2.0 | SAML |
|---|---|
| Authorization framework | XML-based identity federation standard |
| Commonly uses access tokens | Uses SAML assertions |
| Common in APIs and mobile applications | Common in enterprise web SSO |
| Often uses JSON-based tokens | Uses XML-based messages |
| OIDC adds authentication | SAML supports authentication and SSO |

## 21. What is the difference between 401 and 403?

**401 Unauthorized:**

The request does not have valid authentication credentials.

Examples:
- Missing token.
- Invalid token.
- Expired token.

**403 Forbidden:**

The user is authenticated but does not have sufficient permission.

Example:
A normal user tries to access an administrator-only API.

## 22. How would you implement OAuth in a financial services application?



In a financial services application, I would use a centralized identity provider such as Okta to handle authentication and token issuance.

The frontend application would redirect users to the identity provider using OpenID Connect Authorization Code Flow with PKCE.

After successful authentication, the client would receive the appropriate tokens.

For every backend API request, the access token would be validated for signature, issuer, audience, expiration, and permissions.

At the API Gateway, I would enforce authentication, rate limiting, and request routing.

At the microservice level, I would enforce authorization using scopes and roles.

For service-to-service communication, I would use the OAuth Client Credentials Flow, with appropriate service identities and least-privilege scopes.

I would also implement TLS, key rotation, audit logging, monitoring, and secure token handling.

For financial data, I would ensure that users can access only the accounts and transactions they are authorized to view.

## 23. How does OAuth work in an AI agent architecture?



In an enterprise AI agent architecture, OAuth is important for securing access to internal tools and enterprise APIs.

For example, an AI agent may need to retrieve documents from an internal document repository or call a financial services API.

I would use OAuth-based authentication to establish the identity of the calling user or service.

The agent would receive only the permissions necessary to perform its task.

Before invoking a tool, the system would validate the user's authorization and enforce tenant-level and resource-level access controls.

I would avoid giving an LLM unrestricted access to credentials or tokens.

I would also implement audit logging, token expiration, least-privilege scopes, and appropriate human approval for sensitive operations.

---



| Topic | Remember |
|---|---|
| OAuth 2.0 | Authorization |
| OIDC | Authentication layer |
| JWT | Token format |
| Access token | API access |
| Refresh token | Obtain new access token |
| ID token | User identity |
| PKCE | Protects authorization code exchange |
| Scope | Permissions |
| JWKS | Public signing keys |
| Client Credentials | Service-to-service |
| Authorization Code | User authorization flow |
| API Gateway | Entry point and security enforcement |
| 401 | Authentication failure |
| 403 | Insufficient permission |
| mTLS | Mutual certificate-based authentication |



- Okta handles user identity and token issuance.
- OIDC supports user authentication.
- OAuth access tokens authorize API access.
- API Gateway provides centralized security controls.
- Spring Security validates JWTs.
- RBAC and scopes enforce permissions.
- Backend services enforce resource-level authorization.
- TLS and mTLS protect communications where appropriate.

# 10 Questions to Practice

1. Explain OAuth 2.0 with a real-world example.
2. What is the difference between OAuth and OIDC?
3. Explain JWT and its three components.
4. What happens when an access token expires?
5. Explain Authorization Code Flow with PKCE.
6. What is Client Credentials Flow?
7. How does Spring Security validate JWT tokens?
8. How does an API Gateway enforce OAuth security?
9. How would you secure microservices in a financial services application?
10. How would you secure tools and APIs accessed by an AI agent?

**Interview tip:** Be prepared to explain the complete request flow: User → Okta → Token → API Gateway → Spring Boot Microservice → Authorization → Response.
