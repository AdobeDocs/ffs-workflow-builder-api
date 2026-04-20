---
title: Workflow API Feature Guide
description: Learn how the Firefly Creative Production Workflow API lets you execute workflows over multiple assets and manage batch jobs.
hideBreadcrumbNav: true
keywords:
  - firefly creative production
  - workflow
  - batch
  - features
  - feature guide
  - Firefly
  - assets
  - execute
---

# Workflow API feature guide

The Firefly Creative Production Workflow API lets you execute workflows over multiple assets and manage batch jobs. You start a batch with a workflow and a set of images or videos, then monitor progress and retrieve results per asset.

This feature guide offers an overview for using the API endpoints. For the full technical details, see the [API reference](../api/index.md).

## Workflow at a glance

1. **Execute a batch** – Send a workflow and assets to `POST /batch/execute` and receive a `batchId`.
2. **List or filter batches** – Use `GET /batches` to see your batches, optionally filtered by status, workflow, or date.
3. **Check batch status** – Use `GET /batch/{batchId}/status` to see progress, asset counts, and timing.
4. **Cancel (optional)** – Use `POST /batch/{batchId}/cancel` to stop a running or pending batch.
5. **Get execution results** – Use `GET /batch/{batchId}/executions` to list per-asset results, outputs, and errors.

All batch endpoints require authentication (Bearer token). Results are scoped to the authenticated user; you only see batches you created. For more information on authentication, see the [Authentication](../getting-started/index.md) guide.

## API contract version (api-version)

Batch operations use an optional HTTP header **`api-version`** to select the public contract. The current value is **`1.0`**.

| Behavior | Detail |
| -------- | ------ |
| **Omitted header** | The service assumes **`1.0`**. |
| **Supported value** | Send **`api-version: 1.0`** to pin the contract explicitly. |
| **Unsupported value** | The API returns **HTTP 400** with JSON `status: "error"` and a message naming the supported version. |
| **Responses** | Successful and error responses on batch routes include an **`api-version`** response header so clients know which contract handled the call. |

Full parameter and response header definitions are in the [API reference](../api/index.md).

## Execute a batch of assets

Start a new batch with **POST /batch/execute**. You send a workflow definition and the API runs it for each asset in parallel.

**Workflow format requirements:**

- Include a **workflowId** Identifying the unique workflow to be run.
- The **inputs**  array can support multiple arrays of inputs to denote workflow instances in a batch. Each workflow supports defining an array of inputs to be used when running individual workflow instances.

The response is **202 Accepted** and includes a **batchId**, batch **status** (e.g. pending, running), **assets** (counts), **config**, **createdAt**, and **links** for checking the batch status, canceling the batch, and listing the batch executions. Use the returned `batchId` for all later calls.

## List batches

Use **GET /batches** to list batches for the authenticated user. Results are filtered by your user and sorted by creation date (newest first). Pagination and filters are supported.

**Query parameters:**

- **status** – Filter by `pending`, `running`, `completed`, `failed`, or `cancelled`.
- **workflowId** – Filter by workflow ID.
- **createdAfter** / **createdBefore** – Filter by creation time (ISO 8601).
- **limit** – Results per page (1–100, default 50).
- **offset** – Pagination offset (default 0).

The response includes **batches** (array of batch metadata), **pagination** (limit, offset, count, hasMore), and **filters** (the applied query values).

## Get batch status

Use **GET /batch/{batchId}/status** to see current progress and details for a batch.

You get:

- **Overall status** – pending, running, completed, failed, or cancelled.
- **Asset progress** – total, completed, failed, pending, processing.
- **Execution statistics** – e.g. failed count and failed execution IDs for retry.
- **Timing** – estimated time remaining and average execution time per asset.
- **Configuration** – the batch config that was used.

Use this to poll progress or to decide when to fetch execution results or cancel.

## Cancel a batch

Use **POST /batch/{batchId}/cancel** to cancel a batch that is **pending** or **running**. No new assets are started; assets already in progress may still complete. The batch status becomes `cancelled`. You cannot cancel batches that are already completed, failed, or cancelled.

The response includes the **batchId**, **status** (`cancelled`), **message**, **previousStatus**, and **assets** (current counts).

## List execution results

Use **GET /batch/{batchId}/executions** to list per-asset execution results for a batch. You can filter and paginate the results.

Each item includes:

- **Execution status** – pending, running, success, or failed.
- **Input asset** – index, assetId, inputs.
- **Outputs** – result data when status is success.
- **Error** – error message when status is failed.
- **Timing** – startedAt, completedAt, durationMs.

The response also includes **total** (number of assets in the batch), **executions** (array of results), and **pagination** (limit, offset, count, hasMore). Use this to collect outputs or to identify failed executions for retry.

## Next steps

- Try the endpoints with your workflow and assets using the [API reference](../api/index.md).
