---
title: Publish workflows Guide for Firefly Creative Production API
description: Learn how to publish Firefly Creative Production workflows so you can integrate them with other systems or run them at scale using an API.
hideBreadcrumbNav: true
keywords:
  - firefly creative production
  - publish workflow
  - Firefly Creative Production
  - REST API
  - workflow ID
  - programmatic execution
---
# Publish workflows for Firefly Creative Production

Use this guide to understand publishing Firefly Creative Production workflows so you can integrate them with other systems or run them at scale using an API.

Firefly Creative Production allows you to build, integrate, extend, deploy, and run workflows. In the application you can create, run and preview workflows. You can also publish them when you are ready for production use.

Publishing a workflow makes a version of that workflow available for programmatic access. You can integrate that workflow into other systems, platforms, or services, or run it at scale through an API.

## Why publish a workflow?

Publishing creates a version of the workflow that callers can reach through the API. Some key details of published workflows are:

- **Workflow name** — The published workflow keeps the name it had when you published it.
- **Unique workflow ID** — Every workflow receives a unique ID to reference when you call it programmatically (for example, when starting a batch execution).
- **Input parameters** — Each workflow input (like text, image, video, or file inputs) is exposed as an input parameter so you can pass values from your code or integrations.
- For REST API calls, **inputs use primitive types**, including text, content references, and pre-signed URLs for binary inputs.

## How to validate and publish a workflow

To publish a workflow, first build a sequence of nodes in the authoring interface.

- You must **run the workflow successfully at least once** before the publish action is available.
- You must wait for the workflow run to complete. **You cannot publish a running workflow**.
- If you want to give the workflow a specific name, **rename the workflow before publishing**. Publishing uses the workflow’s current name.

When you are satisfied with the workflow, publish it directly from the interface.

## After you publish

When publication has completed, the UI shows a confirmation that includes:

- The **workflow execution endpoint** you use to invoke the workflow.
- A **sample `curl` command** you can run to test the invocation. The sample includes parameters, configuration, and a payload that maps to your workflow’s input nodes.

<InlineAlert variant="warning" slots="text"/>

The sample cURL includes an **authorization token for testing only**. That token remains valid for a short time. For a refreshable token you can use from your own systems, see [Retrieve an access token](../../getting-started/index.md#retrieve-an-access-token) in the authentication guide.

## Invoke the workflow and monitor jobs

To use the published workflow, run the sample cURL command and supply the inputs that are required. Each parameter corresponds to an input node in your workflow.

Programmatic invocations return a job ID and status URLs that you can use to monitor the running workflow and confirm when it completes. For details on status URLs, batch progress, and related operations, see the [Workflow API feature guide](../index.md) and the [API reference](../../api/index.md).

## Next steps

- Run workflows over many assets using the [Workflow API feature guide](../index.md).
- Explore endpoints and schemas in the [API reference](../../api/index.md).
