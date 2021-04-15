
# nginx-openid-connect

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
    * https://f5jaydesai-admin.okta.com/admin/getting-started >> Add App (Use Single Sign-on)

<img src=https://user-images.githubusercontent.com/52437445/114878931-14459000-9e44-11eb-9ee2-de169c3c55fe.png alt="Okta - Add App" width=500>
`Figure 1. Add App in Okta Portal`


  * If your IdP supports OpenID Connect Discovery (usually at the URI `/.well-known/openid-configuration`) then use the `configure.sh` script to complete configuration. In this case you can skip the next section. Otherwise:
    * Obtain the URL for `jwks_uri` or download the JWK file to your NGINX Plus instance
    * Obtain the URL for the **authorization endpoint**
    * Obtain the URL for the **token endpoint**
