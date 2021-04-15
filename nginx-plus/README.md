
# nginx-openid-okta

Reference implementation of NGINX Plus as relying party for OpenID Connect authentication via Okta. Detailed instructions on how you can configure the same are captured in this repository including all the supporting configuration files. 

## Description

This repository describes how to enable OpenID Connect integration for [NGINX Plus](https://www.nginx.com/products/nginx/). The solution depends on NGINX Plus components ([auth_jwt module](http://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html) and [key-value store](http://nginx.org/en/docs/http/ngx_http_keyval_module.html)) and as such is not suitable for [open source NGINX](http://www.nginx.org/en). 


This implementation assumes the following environment:

  * You have created an account on [Okta](https://www.okta.com/) and have access to necessary details.
  * NGINX Plus instance is up and running
  * Backend Application server up and running

With this environment, both the client and NGINX Plus communicate directly with the IdP (Okta) at different stages during the initial authentication event.

For installation and other set-up details, please follow the details on [nginx-openid-connect](https://github.com/nginxinc/nginx-openid-connect) repository. I will not be covering those steps here. 

## Configuring & Collecting details from Okta

  * Create an Application using your Okta Portal
    * https://nginxjdesai-admin.okta.com/admin/getting-started >> Add App (Use Single Sign-on)

   <img src=https://user-images.githubusercontent.com/52437445/114878931-14459000-9e44-11eb-9ee2-de169c3c55fe.png alt="Okta - Add App" width=500>
   
   `Figure 1. Add App in Okta Portal`

   <img src=https://user-images.githubusercontent.com/52437445/114880801-bd40ba80-9e45-11eb-9629-a2f46f3e0c20.png alt="Okta - Create App" width=500>
   
   `Figure 2. Create App in Okta Portal`

   <img src=https://user-images.githubusercontent.com/52437445/114880955-e82b0e80-9e45-11eb-9023-9f139048bfa7.png alt="Okta - App Integration" width=500>
   
   `Figure 3. Create App Integration in Okta Portal`
   
   <img src=https://user-images.githubusercontent.com/52437445/114881220-23c5d880-9e46-11eb-8940-a7127f1f41a8.png alt="Okta - OpenID App Integration" width=500>
   
   `Figure 4. Create OpenID Connect App Integration in Okta Portal`
  
   * Fill **Application name** as you see fit. 
   * Set the **Login redirect URI** to the address of your NGINX Plus instance (including the port number), with `/_codexch` as the path, e.g. `https://nginx-plus-instance.com:443/_codexch`
   
   <img src=https://user-images.githubusercontent.com/52437445/114881428-5a035800-9e46-11eb-9622-a08444a60459.png alt="Okta - ClientID & ClientSecret" width=500>
   
   `Figure 5. Capture Client ID and Client Secret from Okta Portal`
   * Capture **Client ID** value and save for use later. 
   * View/Show **Client secret** value and save for use later.
   

  * Okta supports OpenID Connect Discovery (usually at the URI `/.well-known/openid-configuration`). Access the openid-configuration file and capture the remainder of the values we require to successfully configure our Authentication. 
  * Open in your browser: `https://nginxjdesai.okta.com/.well-known/openid-configuration`

    * Obtain the URL for the **authorization endpoint**
      - In my case "authorization_endpoint":"https://nginxjdesai.okta.com/oauth2/v1/authorize"
    * Obtain the URL for the **token endpoint**
      - In my case "token_endpoint":"https://nginxjdesai.okta.com/oauth2/v1/token"
    * Obtain the URL for the **jwks_uri**
      - In my case "jwks_uri":"https://f5jaydesai.okta.com/oauth2/v1/keys"

## Now, configuring NGINX Plus - will the values we have captured in the previous steps

Manual configuration involves reviewing the following file.

  * **openid_connect_configuration.conf** - this contains the primary configuration for one or more IdPs in `map{}` blocks
    * Modify all of the `map…$oidc_` blocks to match your IdP configuration - to be specific, `$oidc_authz_endpoint`, `$oidc_token_endpoint`, `$oidc_jwt_keyfile`, `$oidc_client` & `$oidc_client_secret`.



