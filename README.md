# Spring-Security-Oauth2-Tutorial

1) Request to http://127.0.0.1:8080/api/users
2) This request is intercepted by client (HelloController)
3) Client will call Ressource server (http://127.0.0.1:8090/api/users) by (WebClient)
4) It will be redirected to http://localhost:9000/login.

![img_2.png](img_2.png)

5) After successful authentication, it will be redirected to authorization server :
http://localhost:9000/oauth2/authorize?response_type=code&client_id=api-client&scope=api.read&state=V3pstWmY8qT_NwW3XHjN7CBBuWJidHm_5cZYG8cGh90%3D&redirect_uri=http://127.0.0.1:8080/authorized
![img.png](img.png)
6) Finally, if the ressource owner givs access to client we will be redirected to  http://127.0.0.1:8080/api/users.

![img_3.png](img_3.png)


# OIDC flow

the client calls: http://localhost:8080/api/hello

=> http://localhost:8080/oauth2/authorization/api-client-oidc

=> http://localhost:9000/oauth2/authorize?response_type=code&client_id=api-client&scope=openid&state=eMlqrai2ZnlsLzFIlB1nCjIfvt2sNYSbeZfUSdeCCBU%3D&redirect_uri=http://127.0.0.1:8080/login/oauth2/code/api-client-oidc&nonce=HNRcADAbprr4Mq7u-gPuVgax_k-5Hr3AiJs5_5a1gVU

=> http://localhost:9000/login

enter login + password

http://localhost:9000/login (302)

=> http://localhost:9000/oauth2/authorize?response_type=code&client_id=api-client&scope=openid&state=D1o5sGQFt1n9J0qa2DH9Zwtj3741s-fFNE6K8tk9jBA%3D&redirect_uri=http://127.0.0.1:8080/login/oauth2/code/api-client-oidc&nonce=bXw-_S-vWKX3U3l9UCo9D1b9NhMy2FPlf38ulTrL8YU

=> http://127.0.0.1:8080/login/oauth2/code/api-client-oidc?code=cpWh8BsBgfPH_94TTjOscy5m7nO9WTn_WyKhmiABdxSwOgLO6RnnSSIBjBYK8c0p3OyOVl6BBRaI_SgXwpamDvP_EouMP63ghLzRyjr9piXelpippS-NauHPoDMpWyjd&state=D1o5sGQFt1n9J0qa2DH9Zwtj3741s-fFNE6K8tk9jBA%3D

=> http://127.0.0.1:8080/api/hello (200)

# OAUTH2 flow:

the client calls: http://127.0.0.1:8080/api/users

=> http://localhost:9000/oauth2/authorize?response_type=code&client_id=api-client&scope=api.read&state=GuYvrPqP5M4uKJoJVcaaEGRW2Nj1nb-liNscNZV6mFI%3D&redirect_uri=http://127.0.0.1:8080/authorized

=> http://localhost:9000/oauth2/authorize

=> http://127.0.0.1:8080/authorized?code=Y9e6_DoAK31aFe0SXGQXt9ztwkkEUayAqjfW_mVwoZq4K_dU5I0EvgppyWvW0JZvQJrYwNX5QrtqrD9Rccu4JSfC-jNPD8o5pWBJ2tUy5R7W4NMDzAfRevVOMjZmcMHk&state=GuYvrPqP5M4uKJoJVcaaEGRW2Nj1nb-liNscNZV6mFI%3D

=> http://127.0.0.1:8080/api/users (200)


# Explanation of URLS:

#### http://localhost:8080/oauth2/authorization/api-client-oidc:

This is the default OIDC url in spring security.

##### http://localhost:9000/oauth2/authorize?response_type=code&client_id=api-client&scope=openid&state=eMlqrai2ZnlsLzFIlB1nCjIfvt2sNYSbeZfUSdeCCBU%3D&redirect_uri=http://127.0.0.1:8080/login/oauth2/code/api-client-oidc&nonce=HNRcADAbprr4Mq7u-gPuVgax_k-5Hr3AiJs5_5a1gVU:

1) http://localhost:9000/oauth2/authorize: This is the base URL for the authorization endpoint of the OAuth 2.0 server. The /oauth2/authorize endpoint is typically used to initiate the OAuth 2.0 authorization flow.

2) response_type=code: This is a query parameter that specifies the desired response type from the authorization server. In this case, it is set to "code", which indicates that the authorization server should return an authorization code that the client can use to request an access token.

3) client_id=api-client: This is a query parameter that specifies the client ID of the OAuth 2.0 client. The client ID is a unique identifier assigned to the client application by the authorization server when the client is registered.

4) scope=openid: This is a query parameter that specifies the scope of the requested authorization. In this case, it is set to "openid", which indicates that the client is requesting access to the OpenID Connect authentication service.

5) state=eMlqrai2ZnlsLzFIlB1nCjIfvt2sNYSbeZfUSdeCCBU%3D: This is a query parameter that specifies an arbitrary value used to maintain state between the client and the authorization server. This value is typically used to prevent cross-site request forgery (CSRF) attacks.

6) redirect_uri=http://127.0.0.1:8080/login/oauth2/code/api-client-oidc: This is a query parameter that specifies the URI to which the authorization server should redirect the user after the user grants or denies authorization. In this case, it is set to "http://127.0.0.1:8080/login/oauth2/code/api-client-oidc", which is the URI where the client is expecting to receive the authorization code.

7) nonce=HNRcADAbprr4Mq7u-gPuVgax_k-5Hr3AiJs5_5a1gVU: This is a query parameter that specifies a nonce value that is used to associate a client session with an ID token. The nonce value should be unique for each authorization request to prevent replay attacks.


#### http://127.0.0.1:8080/login/oauth2/code/api-client-oidc?code=cpWh8BsBgfPH_94TTjOscy5m7nO9WTn_WyKhmiABdxSwOgLO6RnnSSIBjBYK8c0p3OyOVl6BBRaI_SgXwpamDvP_EouMP63ghLzRyjr9piXelpippS-NauHPoDMpWyjd&state=D1o5sGQFt1n9J0qa2DH9Zwtj3741s-fFNE6K8tk9jBA%3D

1) http://127.0.0.1:8080/login/oauth2/code/api-client-oidc: This is the callback URL where the authorization server will redirect the user after the user grants or denies authorization. In this case, it is set to "http://127.0.0.1:8080/login/oauth2/code/api-client-oidc", which is the URI where the client is expecting to receive the authorization code.

2) code=cpWh8BsBgfPH_94TTjOscy5m7nO9WTn_WyKhmiABdxSwOgLO6RnnSSIBjBYK8c0p3OyOVl6BBRaI_SgXwpamDvP_EouMP63ghLzRyjr9piXelpippS-NauHPoDMpWyjd: This is a query parameter that contains the authorization code returned by the authorization server. The client can use this code to request an access token from the token endpoint in order to authenticate and access protected resources on behalf of the user.

3) state=D1o5sGQFt1n9J0qa2DH9Zwtj3741s-fFNE6K8tk9jBA%3D: This is a query parameter that contains the state value that was originally sent in the authorization request. The state value is used to maintain state between the client and the authorization server and prevent cross-site request forgery (CSRF) attacks. The value is URL-encoded in this case with %3D representing the "=" character.

#### http://127.0.0.1:8080/authorized?code=Y9e6_DoAK31aFe0SXGQXt9ztwkkEUayAqjfW_mVwoZq4K_dU5I0EvgppyWvW0JZvQJrYwNX5QrtqrD9Rccu4JSfC-jNPD8o5pWBJ2tUy5R7W4NMDzAfRevVOMjZmcMHk&state=GuYvrPqP5M4uKJoJVcaaEGRW2Nj1nb-liNscNZV6mFI%3D

This URL that might be used as a redirect URI after a successful authorization process with an OAuth 2.0 authorization server.

1) code: This parameter contains an authorization code. The authorization code is a temporary code that is generated by the authorization server in response to a successful authorization request by an OAuth 2.0 client. The client can then exchange this code for an access token, which can be used to make protected resource requests on behalf of the authorized user.

2) state: This parameter contains a state value that is typically used for security purposes to protect against cross-site request forgery (CSRF) attacks. The state value is generated by the client and included in the authorization request, and it should be returned unchanged by the authorization server in the redirect URI. The client can then verify that the returned state value matches the original value to ensure that the authorization response is legitimate.