# OAuth 2.0 – Business-to-Business (B2B) Security and Service-to-Service (S2S) Security

## 1. What is OAuth 2.0?

OAuth 2.0 is an authorization framework that allows an application or service to access protected resources using an access token.

Instead of sharing usernames and passwords, applications obtain an access token from an Authorization Server and use it to access an API.

OAuth 2.0 answers the question:

**Is this application authorized to access this resource?**

---

## 2. Business-to-Business (B2B) Security

### What is B2B security?

B2B security protects communication between two different organizations.

For example:

- Bank A wants to access an API exposed by Financial Institution B.
- Bank A needs permission to access specific APIs from Financial Institution B.
- OAuth 2.0 can be used to authenticate the client application and authorize access.

### Architecture

```text
Organization A                         Organization B
+----------------+                     +----------------------+
|                |                     |                      |
| Client App     |                     | Authorization Server |
|                |                     |                      |
+-------+--------+                     +----------+-----------+
        |                                         |
        | 1. Request access token                 |
        |    Client ID + Secret                   |
        +---------------------------------------->|
        |                                         |
        | 2. Access Token                         |
        |<----------------------------------------+
        |
        | 3. Call protected API
        |    Authorization: Bearer <token>
        +---------------------------------------->
                                                  |
                                                  v
                                         +-------------------+
                                         | Resource Server   |
                                         |                   |
                                         | Validates Token   |
                                         | Checks Scope      |
                                         +-------------------+
```

### OAuth 2.0 flow

For machine-to-machine B2B integration, the Client Credentials Grant is commonly used.

1. Organization A registers its application with Organization B.
2. Organization B provides a Client ID and Client Secret.
3. Organization A requests an access token.
4. Authorization Server validates the client credentials.
5. Authorization Server issues an access token.
6. Organization A calls the protected API with the token.
7. Resource Server validates the token and checks permissions.

---

## 3. Service-to-Service (S2S) Security

### What is S2S security?

Service-to-Service security protects communication between two backend services, usually within an organization or distributed application.

For example, consider a loan processing application.

- Loan Service needs to call Payment Service.
- Payment Service exposes protected REST APIs.
- Loan Service must obtain an access token before calling Payment Service.

### Architecture

```text
             Application
                  |
                  v
             Loan Service
                  |
                  | 1. Request access token
                  v
          Authorization Server
                  |
                  | 2. Access Token
                  v
             Loan Service
                  |
                  | 3. Call Payment API
                  |    Bearer Token
                  v
             Payment Service
                  |
                  | 4. Validate Token
                  | 5. Check Scope
                  v
             Process Request
```

### OAuth 2.0 flow

1. Loan Service is registered as an OAuth client.
2. It receives a Client ID and Client Secret.
3. Loan Service requests an access token using Client Credentials Grant.
4. Authorization Server validates the client.
5. Authorization Server issues an access token.
6. Loan Service calls Payment Service with the token.
7. Payment Service validates the token and permissions.
8. Payment Service processes the request if authorized.

---

## 4. B2B vs S2S Security

| Feature | B2B Security | S2S Security |
|---|---|---|
| Communication | Between organizations | Between backend services |
| Example | Bank A to Bank B | Loan Service to Payment Service |
| Authentication | OAuth 2.0 | OAuth 2.0 |
| Common grant type | Client Credentials | Client Credentials |
| Client ID | Registered application identity | Registered service identity |
| Client Secret | Application credential | Service credential |
| Access Token | Authorizes API access | Authorizes API access |
| Scope | Defines permitted API access | Defines permitted service operations |

**Important:** B2B and S2S describe the communication context, not different OAuth grant types. Both can use the same OAuth 2.0 Client Credentials flow.

---

## 5. Important OAuth 2.0 Components

### 5.1 Client

The application or service requesting access to another service.

Example:

Loan Service is the OAuth client.

### 5.2 Client ID

A unique identifier assigned to a registered application.

Example:

```text
client_id = loan-service-client
```

The Client ID identifies the application. It is not a password.

### 5.3 Client Secret

A confidential credential used by the client to authenticate itself to the Authorization Server.

Example:

```text
client_secret = <secret-value>
```

The Client Secret must be stored securely, such as in AWS Secrets Manager or Azure Key Vault.

Never expose it in frontend applications or source code.

### 5.4 Authorization Server

The server responsible for authenticating OAuth clients and issuing access tokens.

Examples:

- Microsoft Entra ID
- Okta
- Keycloak
- Auth0

### 5.5 Access Token

A credential that the client sends to the Resource Server to access protected APIs.

Example:

```http
Authorization: Bearer eyJhbGciOi...
```

An access token may be a JWT or an opaque token.

### 5.6 Scope

A scope defines what permissions the client is requesting.

Example:

```text
payment.read
payment.write
loan.read
loan.write
```

If Loan Service requests:

```text
payment.read
```

It should only receive permission to perform the operations authorized by that scope.

The API must enforce the required permissions.

### 5.7 Resource Server

The API that receives the access token and validates whether the caller has permission to access the requested resource.

Example:

Payment Service is the Resource Server.

---

## 6. How to Obtain Client ID, Client Secret, and Scope

These values are normally configured when an application is registered with an Identity Provider.

Example using Microsoft Entra ID:

### Step 1: Register the application

1. Open Microsoft Entra admin center.
2. Navigate to App registrations.
3. Select New registration.
4. Provide the application name.
5. Register the application.

### Step 2: Get Client ID

Open the registered application and locate:

```text
Application (client) ID
```

### Step 3: Get Client Secret

1. Open Certificates & secrets.
2. Select New client secret.
3. Set an expiration period.
4. Copy the generated secret value securely.

The secret value is displayed when created. Store it immediately.

### Step 4: Configure API permissions and scopes

For an API you own:

1. Register the API application.
2. Open Expose an API.
3. Define an application permission, such as:

```text
payment.read
```

4. Configure the client application to request that permission.
5. Grant the required administrator consent.

For client credentials, these are application permissions rather than delegated user permissions.

---

## 7. Request an Access Token

Example using OAuth 2.0 Client Credentials Grant.

### Token endpoint

```http
POST https://identity.example.com/oauth2/token
```

### Request

```http
POST /oauth2/token HTTP/1.1
Host: identity.example.com
Content-Type: application/x-www-form-urlencoded
Authorization: Basic <base64-client-credentials>

grant_type=client_credentials&scope=payment.read
```

The exact authentication method and token URL depend on the Identity Provider.

### Response

```json
{
  "access_token": "eyJhbGciOi...",
  "token_type": "Bearer",
  "expires_in": 3600,
  "scope": "payment.read"
}
```

The client should cache the token and request another one when it expires, rather than obtaining a new token for every API request.

---

## 8. Call the Protected API

Once the client receives an access token, it calls the Payment Service.

### Request

```http
GET /api/payments/1001 HTTP/1.1
Host: payment.example.com
Authorization: Bearer eyJhbGciOi...
```

The Payment Service validates:

1. Token signature or introspection result.
2. Token expiration.
3. Issuer (`iss`).
4. Audience (`aud`).
5. Required scope or application role.

If valid and authorized, the API processes the request.

---

## 9. OAuth 2.0 Implementation in Spring Boot

### Resource Server Configuration

Payment Service acts as an OAuth Resource Server.

Add the Spring Boot dependency:

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
          issuer-uri: https://identity.example.com/
```

Spring Security uses the configured issuer to discover the authorization server's public signing keys and validate JWTs.

### Security Configuration

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(
            HttpSecurity http) throws Exception {

        return http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/payments/**")
                .hasAuthority("SCOPE_payment.read")
                .anyRequest()
                .authenticated()
            )
            .oauth2ResourceServer(oauth2 ->
                oauth2.jwt(Customizer.withDefaults())
            )
            .build();
    }
}
```

### Payment Controller

```java
@RestController
@RequestMapping("/api/payments")
public class PaymentController {

    @GetMapping("/{id}")
    public String getPayment(@PathVariable String id) {
        return "Payment details for " + id;
    }
}
```

Spring Security validates the token before allowing the request to reach the controller.

---

## 10. Important Security Concepts

### Authentication vs Authorization

| Authentication | Authorization |
|---|---|
| Who are you? | What are you allowed to do? |
| Verifies identity | Verifies permissions |
| Client ID and Secret authenticate the OAuth client | Scopes and roles control API access |

### JWT

JSON Web Token is a token format that can carry claims.

A JWT commonly contains:

```json
{
  "iss": "https://identity.example.com/",
  "sub": "loan-service-client",
  "aud": "payment-api",
  "scope": "payment.read",
  "exp": 1893456000
}
```

Important claims:

- `iss`: Token issuer.
- `sub`: Subject represented by the token.
- `aud`: Intended recipient.
- `exp`: Expiration time.
- `scope`: Granted permissions, when included.

A JWT is typically signed. Base64 encoding is not encryption, so do not place secrets in JWT claims.

### mTLS (Mutual TLS)

mTLS provides certificate-based authentication between two services.

In regular TLS, the server presents a certificate to the client.

In mutual TLS, both client and server authenticate using certificates.

mTLS can be used alongside OAuth 2.0 for additional security.

### Least Privilege

Give each service only the permissions it needs.

For example:

- Loan Service: `payment.read`
- Accounting Service: `payment.read`, `payment.reconcile`
- Admin Service: explicitly approved administrative permissions

### Client Secret Management

Never hardcode credentials.

Use:

- AWS Secrets Manager
- Azure Key Vault
- Kubernetes Secrets with appropriate encryption and access controls

Rotate secrets regularly and prefer stronger credential options such as private-key JWT or mTLS where supported.

---

## 11. Summary

```text
OAuth 2.0 Client Credentials Flow

Client Application
       |
       | Client ID + Client Secret
       v
Authorization Server
       |
       | Access Token
       v
Client Application
       |
       | Bearer Token
       v
Resource Server / API
       |
       | Validate Token
       | Check Scope / Role
       v
Access Granted or Denied
```

Key takeaways:

- B2B: Communication between different organizations.
- S2S: Communication between backend services.
- OAuth 2.0: Authorization framework.
- Client ID: Identifies the registered client.
- Client Secret: Authenticates the confidential client.
- Scope: Defines the permissions requested and granted.
- Authorization Server: Issues access tokens.
- Resource Server: Validates tokens and enforces access.
- JWT: A common access-token format.
- mTLS: Provides mutual certificate-based authentication.
- Client Credentials Grant: Common OAuth flow for machine-to-machine communication.

**Remember:** OAuth 2.0 is primarily an authorization framework. For user authentication, OpenID Connect (OIDC) builds on top of OAuth 2.0.
