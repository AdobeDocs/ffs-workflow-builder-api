---
title: Workflow Builder API
description: Run published workflows over multiple assets at scale with batch execution, progress tracking, and per-asset results.
hideBreadcrumbNav: true
keywords:
  - Workflow Builder
  - batch
  - workflow
  - Firefly
  - assets
  - API
---

<Superhero slots="image, heading, text" background="rgb(119, 151, 186)" theme="dark" />

![Workflow Builder API Hero](workflow-builder-hero.png)

# Workflow Builder API

Run published workflows over many assets at scale with batch execution, progress tracking, and per-asset results.

## Overview

The Workflow Builder API (Firefly Creative Production for Enterprise) lets you execute a single published workflow across multiple assets in one batch. You submit a workflow definition and your images or videos; the service processes each asset through the workflow in parallel and returns results per asset.

Use it to remove backgrounds, apply effects, or run any published workflow at scale with full control over concurrency, priority, and error handling.

### What is the Workflow Builder API?

The Workflow Builder API allows you to:

* **Integrate systems** — Connect the Workflow Builder API with external platforms to trigger workflow executions directly from those environments. This enables automated creative production or modification of assets using content that already exists in those systems, with processed outputs returned back to the originating environment for seamless end-to-end workflows.
* **Execute batches** — Start a batch job with a workflow ID and full workflow definition (actions and connections); include all images in the workflow's input-images action and receive a batch ID to track progress.
* **List and filter batches** — View your batches, filtered by status, workflow ID, or creation date, with pagination.
* **Monitor progress** — Get batch status with asset counts (total, completed, failed, pending, processing), execution statistics, and estimated time remaining.
* **Cancel when needed** — Cancel a pending or running batch so no new assets are started; in-flight work may complete.
* **Retrieve results** — List individual execution results per asset, including outputs for successes and error messages for failures, with optional filtering and pagination.

### Why choose Workflow Builder?

* **Scale without rewriting** — Run the same published workflow you use for single assets across dozens or hundreds of assets in one request.
* **Controlled parallelism** — Set concurrency and priority for fair-share throttling and predictable throughput.
* **Resilient runs** — Continue on error by default so one failing asset does not stop the batch; use execution lists to identify and retry failures.
* **Full visibility** — Track progress with asset counts and timing estimates, and fetch detailed per-asset results when the batch completes.

## Discover

<DiscoverBlock slots="heading, link, text"/>

### Get started

[Workflow Builder feature guide](guides/index.md)

Learn the workflow: execute a batch, list and filter batches, check status, cancel jobs, and retrieve execution results.

<DiscoverBlock slots="link, text"/>

[API reference](api/index.md)

Explore endpoints, request and response schemas, and try the API.

## Ready to try it?

See the [Workflow Builder feature guide](guides/index.md) for step-by-step use of the API, and [Getting started](getting-started/index.md) for authentication and setup.
