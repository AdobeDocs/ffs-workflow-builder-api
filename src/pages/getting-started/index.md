---
title: Firefly Creative Production Workflow API Authentication
description: Learn how to set up and authenticate a project to use the Firefly Creative Production Workflow API.
hideBreadcrumbNav: true
keywords:
- authentication
- access token
- OAuth
- Adobe IMS
- API security
---
# Authentication

Learn how to authenticate requests to the Firefly Creative Production Workflow API.

## Overview

Every request made to the Firefly Creative Production Workflow API must include an encrypted access token.

Your secure, server-side application retrieves an access token by making a request to the [Adobe Identity Management System (IMS)](https://www.adobe.com/content/dam/cc/en/trust-center/ungated/whitepapers/corporate/adobe-identity-management-services-security-overview.pdf) with your **Client ID** and **Client Secret**.

## Prerequisites

This tutorial assumes you have worked with your Adobe representative and have the following:

- An [Adobe Developer Console](https://developer.adobe.com/) account.
- A [project](https://developer.adobe.com/developer-console/docs/guides/projects/projects-empty/) with Firefly Creative Production Workflow API [OAuth Server-to-Server credentials set up](https://developer.adobe.com/developer-console/docs/guides/services/services-add-api-oauth-s2s/).
- Access to your **Client ID** and **Client Secret** from the [Adobe Developer Console project](https://developer.adobe.com/developer-console/docs/guides/services/services-add-api-oauth-s2s/#api-overview). Securely store these credentials and never expose them in client-side or public code.

## Retrieve an access token

A temporary access token validates calls to the API. You can [generate an access token directly in the Developer Console](https://developer.adobe.com/developer-console/docs/guides/services/services-add-api-oauth-s2s#generate-token), or programmatically by following the steps below.

1. Open a secure terminal and export your **Client ID** and **Client Secret** as environment variables so that later commands can access them:

  ```bash
  export WFB_CLIENT_ID=<Your_Client_ID>
  export WFB_CLIENT_SECRET=<Your_Client_Secret>
  ```

2. Run the following command to generate an access token:

  ```bash
  curl --location 'https://ims-na1.adobelogin.com/ims/token/v3' \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'grant_type=client_credentials' \
  --data-urlencode "client_id=WFB_CLIENT_ID" \
  --data-urlencode "client_secret=WFB_CLIENT_SECRET" \
  --data-urlencode 'scope=openid,AdobeID,session,additional_info,read_organizations,firefly_api,ff_apis'
  ```

  The response will look like this:

  ```json
  {
      "access_token": "exampleAccessTokenAsdf123",
      "token_type": "bearer",
      "expires_in": 86399
  }
  ```

  The response includes an `expires_in` field that specifies the length of time, in seconds, that the access token is valid. After the access token expires, your application must request a new token.

3. Export your access token as an environment variable:

  ```bash
  export WFB_ACCESS_TOKEN=<Your_Access_Token>
  ```
