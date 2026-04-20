---
title: Project Setup
description: Learn what to configure before using Workflow Builder API.
hideBreadcrumbNav: true
keywords:
  - Workflow Builder API
  - developer console
  - Adobe Developer Console
  - cloud storage
  - batch
  - workflow
---

# Project setup

This page outlines prerequisites for using the Workflow Builder API.

Before you call the API, you should:

1. **Configure access in Adobe Developer Console** — Create or use a project with Workflow Builder API OAuth Server-to-Server credentials. See [Authentication](../index.md) for how to retrieve access tokens and which scopes to request.

2. **Prepare asset locations** — Your workflow definition includes input images or videos (for example in an `input-images` action). Assets must be reachable at HTTPS URLs that the service can access. Depending on your cloud storage (for example Amazon S3 or Azure Blob), you may use pre-signed URLs or other secure access patterns that fit your environment.

3. **Send required headers on each request** — Batch endpoints require authentication (API key or Bearer token) and headers such as `x-gw-ims-org-id`, `x-gw-ims-user-id`, and `x-api-key`.

<InlineAlert variant="warning" slots="text" />

Asset URLs must be valid for the lifetime of the batch and must allow the Workflow Builder service to retrieve your inputs according to your security model (for example URL expiration and network rules).

## Set up cloud storage

When you call the API, the workflow and batch requests include asset URLs (for example in the workflow’s `input-images` action). If you use object storage, create buckets or containers for your source files and expose them via HTTPS URLs as required by your integration (for example pre-signed GET URLs).

You can use common cloud storage platforms listed in the [Workflow Builder API usage notes](../usage/index.md#supported-storage-solutions).

If they do not already exist, create containers in your storage platform for the assets you pass into the workflow. Ensure each URL you embed in the batch request resolves to the correct object for the duration of processing.
