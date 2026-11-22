# OAuth 2.0, OpenID Connect (OIDC) and JWT 

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
