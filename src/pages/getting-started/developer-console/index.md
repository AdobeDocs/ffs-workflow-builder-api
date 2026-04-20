---
title: Using Adobe Developer Console
description: Learn how to use Adobe Developer Console to manage resources and credentials for authenticating with Workflow Builder API.
hideBreadcrumbNav: true
keywords:
  - Adobe Developer Console
  - Workflow Builder API
  - OAuth 2.0
  - access tokens
  - client ID
  - client secret
  - server-to-server authentication
  - secret rotation
  - I/O Management API
---

# Adobe Developer Console

Use Adobe Developer Console to manage resources and credentials.

## Overview

The **Adobe Developer Console** is an administration interface that enables developers to manage Adobe APIs and services. To securely access Adobe APIs, including the Workflow Builder API, your application must authenticate using OAuth 2.0 protocols. This involves obtaining an access token that grants your application permission to interact with Adobe services.

## Getting started for Workflow Builder API

You will need:

- An [Adobe Developer Console](https://developer.adobe.com/) account.
- A [project](https://developer.adobe.com/developer-console/docs/guides/projects/projects-empty/) with Workflow Builder API [OAuth Server-to-Server credentials set up](https://developer.adobe.com/developer-console/docs/guides/services/services-add-api-oauth-s2s/).
- Access to your **Client ID** and **Client Secret** from the Developer Console.

## Access tokens

You can [generate access tokens](https://developer.adobe.com/developer-console/docs/guides/services/services-add-api-oauth-s2s/#api-overview) from the Developer Console UI or programmatically.

To generate an access token programmatically, follow the [Authentication](../index.md) guide.

## Best practices

- **Token management:** Access tokens are valid for 24 hours. Implement a mechanism to refresh tokens before they expire to maintain uninterrupted access.
- **Scope limitation:** Request only the scopes your application needs to follow the principle of least privilege.
- **Secure storage:** Store your Client ID, Client Secret, and access tokens securely to prevent unauthorized access.

## Managing secrets

### Rotating secrets

You can rotate your client secret as needed. For some teams, that means rotating client secrets every few months. For others, it may mean rotating secrets every day.

Rotating your secret is recommended if a credential is leaked or accessed without authorization. Rotating client secrets periodically is also a common security practice. Similar to access tokens, you can rotate your client secret in the Developer Console UI or by using an API.

**Org admins:** In Adobe Developer Console, open your project. Choose **Add to Project**, select **API**, and add **I/O Management API** to your project. That API lets your credential read, delete, and generate new client secrets. Configure the credential name before you save it.

To rotate secrets without contacting the org admin, developers need the following:

- `orgId`
- `projectId`
- `credentialId`
- Access token

Open the OAuth Server-to-Server credential overview page. Copy `orgId`, `projectId`, and `credentialId` from the URL by comparing it to this pattern:

```bash
https://developer.adobe.com/console/projects/{orgId}/{projectId}/credentials/{credentialId}/details/oauthservertoserver
```

Next, obtain an access token. Use the following command. Include the scopes that the I/O Management API requires in the `scope` parameter:

```bash
curl -X POST 'https://ims-na1.adobelogin.com/ims/token/v3' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&grant_type=client_credentials&scope=AdobeID,openid,read_organizations,additional_info.projectedProductContext,additional_info.roles,adobeio_api,read_client_secret,manage_client_secrets'
```

### List all secrets

```bash
curl -X GET 'https://api.adobe.io/console/organizations/{orgId}/credentials/{credentialId}/secrets' \
     -H 'Authorization: Bearer {ACCESS_TOKEN}' \
     -H 'x-api-key: {CLIENT_ID}'
```

### Generate a new secret

Make the following request. The API response contains the `client_secret` that was added to the project and its `uuid`. You can view the secret again later in the Developer Console UI. After you generate a new secret, update your application to use the new secret.

```bash
curl -X POST 'https://api.adobe.io/console/organizations/{orgId}/credentials/{credentialId}/secrets' \
     -H 'Authorization: Bearer {ACCESS_TOKEN}' \
     -H 'x-api-key: {CLIENT_ID}'
```

### Delete a client secret

To delete a secret, first call the API to list all client secrets for your credential. Find the `uuid` value for the older secret you want to remove. Call the following API to delete that `client_secret` from your credential by passing the `uuid` in the URL.

```bash
curl -X DELETE 'https://api.adobe.io/console/organizations/{orgId}/credentials/{credentialId}/secrets/{uuid}' \
     -H 'Authorization: Bearer {ACCESS_TOKEN}' \
     -H 'x-api-key: {CLIENT_ID}'
```

<InlineAlert variant="warning" slots="text" />

After a client secret is deleted, you cannot restore it. Confirm you have replaced the old client secret everywhere before you delete it.
