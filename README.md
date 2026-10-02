# Awesome Cloud Emulators [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Cloud emulators reproduce selected cloud service APIs and behavior for local development and automated testing.

Discover emulators and cloud-specific test doubles for **AWS, Microsoft Azure, Google Cloud (GCP), Oracle Cloud, Cloudflare, Snowflake, and European cloud providers**. Build repeatable integration tests and shorten feedback loops without provisioning every dependency in a cloud account.

Local mocks and emulators can **reduce development and CI costs** by avoiding repeated provisioning, idle test resources, and billable service calls in cloud accounts. Savings depend on the workload and should be weighed against local compute, maintenance, and any emulator licensing costs.

Entries are grouped by the APIs they emulate, not where they run. Multi-service means a suite or collection spanning services; it does not promise integrated behavior between them. Provider-built, community, and commercial tools are included. Supporting tools are listed separately.

Within each section, open-source tools come first, followed by other distributions, then paid/commercial tools; entries are alphabetized within each group. “Other distributions” includes free vendor binaries, mixed suites, and tools whose runtime source license has not been established. A usable open-source edition stays in the first group even when optional paid editions exist.

**Emulation is not full service parity.** Check supported operations, persistence, identity behavior, runtime requirements, licensing, and access conditions. Use real-cloud tests for production-specific guarantees. The [selection guide](docs/selection-guide.md) includes scenario-based shortlists, a comparison of every entry, and an evaluation checklist.

**Focus and independence:** This list prioritizes open-source cloud emulators and supporting tools. Commercial products are included where relevant to help readers compare options; inclusion does not imply endorsement or a recommendation to purchase. Evaluate each tool’s capabilities, licensing, costs, and suitability for your needs.

**License markers:** 💰 Paid commercial plan or license for commercial use; a free tier or exception may exist. 📜 Vendor-specific software terms or an EULA apply; this does **not** mean a fee is required. Open-source licenses still apply to unmarked tools. Markers highlight verified conditions, not an exhaustive license audit.

See [license and access notes](docs/selection-guide.md#license-and-access-notes) for the marked tools and primary sources.

## Contents

- [AWS](#aws)
  - [AWS Multi-Service](#aws-multi-service)
  - [AWS Single-Service](#aws-single-service)
- [Microsoft Azure](#microsoft-azure)
  - [Azure Multi-Service](#azure-multi-service)
  - [Azure Single-Service](#azure-single-service)
- [Google Cloud and Firebase](#google-cloud-and-firebase)
  - [Google Cloud Multi-Service](#google-cloud-multi-service)
  - [Google Cloud Single-Service](#google-cloud-single-service)
- [Oracle Cloud](#oracle-cloud)
- [Cloudflare](#cloudflare)
- [Cross-Cloud](#cross-cloud)
- [Beyond Emulation](#beyond-emulation)
- [Snowflake](#snowflake)
- [Supporting Tools](#supporting-tools)
- [Choosing an Emulator](#choosing-an-emulator)
- [Support](#support)

## AWS

### AWS Multi-Service

**Open source**

- [fakecloud](https://fakecloud.dev) - Local AWS API emulator with test SDKs for inspecting effects, resetting state, and controlling asynchronous processors.
- [Floci](https://floci.io/floci) - Community AWS emulator that exposes multiple service APIs through a shared local endpoint.
- [Hiraeth](https://github.com/SethPyle376/hiraeth) - Rust-based AWS emulator focused on SQS and partial SNS/IAM APIs, with SQLite persistence and a web UI for inspecting requests and local state.
- [LocalEmu](https://localemu.cloud) - Python-based AWS emulator with persistent local state and Docker-backed execution for selected services.
- [MiniStack](https://ministack.org) - Community AWS emulator with multi-account and multi-region support and optional engine-backed services.
- [Moto](https://github.com/getmoto/moto) - Community AWS mocking library for Python tests, with a standalone server mode for other SDKs and languages.

**Paid/commercial**

- [LocalStack 💰 📜](https://docs.localstack.cloud/aws) - Vendor-maintained AWS emulation platform distributed as a container; commercial use requires an appropriate paid plan, subject to vendor exceptions; a free non-commercial Hobby plan is available and activation requires authentication.

### AWS Single-Service

**Open source**

- [ElasticMQ](https://github.com/softwaremill/elasticmq) - Community message queue with an Amazon SQS-compatible interface, usable as a standalone server or embedded dependency.
- [S3Mock](https://github.com/adobe/S3Mock) - Community implementation of a subset of the Amazon S3 API for local integration testing, with Docker and Testcontainers support.
- [S3Proxy](https://github.com/gaul/s3proxy) - Community S3-compatible API server backed by configurable storage, including a local filesystem for testing object operations.

**Other distributions**

- [DynamoDB Local 📜](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.html) - AWS-provided local DynamoDB implementation for developing and testing database interactions.


## Microsoft Azure

### Azure Multi-Service

**Open source**

- [Floci AZ](https://github.com/floci-io/floci-az) - Community Azure emulator exposing multiple service APIs, with Docker-backed execution for selected services.
- [miniblue](https://github.com/moabukar/miniblue) - Community Go-based Azure emulator that runs multiple services behind a single port, with no Azure account required and optional real backends such as PostgreSQL and Redis.
- [Topaz](https://github.com/TheCloudTheory/Topaz) - Community Azure emulator covering control and data plane APIs, with local ARM and Bicep template deployments, Azure RBAC, and Microsoft Entra ID tenant emulation.

**Paid/commercial**

- [LocalStack for Azure 💰 📜](https://docs.localstack.cloud/azure) - Vendor-maintained Azure emulator distributed as a container, covering Resource Manager and selected data plane APIs; it is in private preview with access enabled on request, commercial use requires an appropriate paid plan, and activation requires an auth token.

### Azure Single-Service

**Open source**

- [Azure Key Vault Emulator](https://github.com/james-gould/azure-keyvault-emulator) - Community Key Vault emulator for testing Azure SDK clients locally, with Docker and .NET Aspire integration.
- [Azurite](https://github.com/Azure/Azurite) - Open-source Azure Storage emulator from Microsoft for Blob, Queue, and Table service development and testing.

**Other distributions**

- [Azure Cosmos DB Emulator](https://learn.microsoft.com/en-us/azure/cosmos-db/emulator) - Microsoft-provided local Cosmos DB environment; supported APIs and features vary by emulator variant and platform.
- [Azure Event Hubs Emulator 📜](https://learn.microsoft.com/en-us/azure/event-hubs/overview-emulator) - Microsoft-provided local environment for developing and testing Event Hubs producers and consumers.
- [Azure Service Bus Emulator 📜](https://learn.microsoft.com/en-us/azure/service-bus-messaging/overview-emulator) - Microsoft-provided local Service Bus environment for testing messaging applications in isolation.


## Google Cloud and Firebase

### Google Cloud Multi-Service

**Open source**

- [Floci GCP](https://floci.io/floci-gcp) - Community Google Cloud emulator covering services such as Cloud Storage, Pub/Sub, and Firestore through local APIs.
- [Fullstory Emulators](https://github.com/fullstorydev/emulators) - Community collection of Bigtable and Cloud Storage emulators, usable as Go libraries or standalone servers with optional persistence.

**Other distributions**

- [Firebase Local Emulator Suite](https://firebase.google.com/docs/emulator-suite) - Google-provided suite for local testing across Firebase services, including Authentication, Firestore, Realtime Database, and Cloud Functions.

### Google Cloud Single-Service

**Open source**

- [BigQuery Emulator](https://github.com/goccy/bigquery-emulator) - Community BigQuery API emulator implemented in Go for local database and query integration tests.
- [Bigtable Emulator](https://cloud.google.com/bigtable/docs/emulator) - Google-provided local Bigtable environment for developing and testing client applications.
- [Cloud Tasks Emulator](https://github.com/aertje/cloud-tasks-emulator) - Community Cloud Tasks emulator for local queue and task-dispatch testing.
- [Fake GCS Server](https://github.com/fsouza/fake-gcs-server) - Community Google Cloud Storage emulator usable as a standalone server or Go testing library.
- [Pub/Sub pstest](https://pkg.go.dev/cloud.google.com/go/pubsub/v2/pstest) - In-process fake Pub/Sub server from the Google Cloud Go client libraries for focused Go tests.
- [Spanner Emulator](https://cloud.google.com/spanner/docs/emulator) - Google-provided local Spanner environment for testing application behavior against supported database APIs.

**Other distributions**

- [Datastore Emulator](https://cloud.google.com/datastore/docs/tools/datastore-emulator) - Google-provided local environment for testing applications that use the Datastore API.
- [Firestore Emulator](https://cloud.google.com/firestore/native/docs/emulator) - Google-provided local Firestore environment for testing database operations without connecting to a production database.
- [Pub/Sub Emulator](https://cloud.google.com/pubsub/docs/emulator) - Google-provided local Pub/Sub environment for testing publishers and subscribers.


## Oracle Cloud

**Open source**

- [Floci OCI](https://github.com/floci-io/floci-oci) - Community Oracle Cloud Infrastructure emulator covering identity, object storage, messaging, and selected other APIs, with configurable persistence and optional container-backed execution.

## Cloudflare

**Open source**

- [Miniflare](https://developers.cloudflare.com/workers/testing/miniflare/) - Cloudflare-maintained local Workers simulator with storage bindings such as KV, R2, D1, and Durable Objects, also used by Wrangler for local development.

## Cross-Cloud

**Open source**

- [cloudemu](https://github.com/stackshy/cloudemu) - In-memory simulation of AWS, Azure, and Google Cloud APIs, runnable as a server or embedded in Go tests.

- [Feint](https://github.com/stephrobert/feint) - Community emulator for Scaleway, Outscale, and Exoscale APIs, with official CLI and Terraform/OpenTofu workflows and optional Incus-backed machine execution.

**Other distributions**

- [Vera](https://github.com/project-vera/vera) - Local simulation of AWS EC2 and Google Compute APIs for infrastructure automation tests.


## Beyond Emulation

Some infrastructure tests need real guest execution, storage, and networking behind cloud-compatible APIs. The platforms here run workloads on your own infrastructure; they are alternatives to API emulators, with different host requirements and costs.

**Open source**

- [Spinifex 💰](https://github.com/mulgadc/spinifex) - Self-hosted AWS-compatible platform for testing infrastructure and workloads with real QEMU/KVM instances, storage, and OVN-backed networking.


## Snowflake

**Paid/commercial**

- [LocalStack for Snowflake 💰 📜](https://docs.localstack.cloud/snowflake) - Vendor-maintained Snowflake emulator distributed as a container for local SQL, data pipeline, and integration testing; activation requires an auth token and an assigned Snowflake license, available through a trial or paid offering.


## Supporting Tools

These tools run local workloads, manage emulator lifecycles, or prepare and restore test baselines; they are not full cloud emulators. Checkpoint integrations are listed for their specific emulator use case.

**Open source**

- [AWS Lambda Runtime Interface Emulator](https://github.com/aws/aws-lambda-runtime-interface-emulator) - AWS-provided proxy for testing Lambda functions packaged as container images locally; it does not emulate dependent cloud services.
- [AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/using-sam-cli-local-testing.html) - AWS tooling for local invocation and debugging of serverless applications, including Lambda functions.
- [Azure Functions Core Tools](https://learn.microsoft.com/en-us/azure/azure-functions/functions-run-local) - Microsoft tooling to run and test Azure Functions locally; configure service dependencies separately.
- [CRIU](https://criu.org) - Linux process checkpoint/restore utility for capturing a running emulator and resuming its in-memory state; requires a compatible kernel and runtime environment.
- [DMTCP](https://github.com/dmtcp/dmtcp) - User-space checkpointing for Linux applications launched under its control; a candidate for emulator-process experiments after compatibility validation.
- [Docker Checkpoint/Restore](https://docs.docker.com/reference/cli/docker/checkpoint) - Experimental Docker Engine integration with CRIU for checkpointing and restoring running containers.
- [Moto Recorder](https://docs.getmoto.org/en/stable/docs/configuration/recorder/index.html) - Built-in Moto request recording and replay for rebuilding test baselines; replay is not a process-memory snapshot.
- [Podman Checkpoint/Restore](https://podman.io/docs/checkpoint) - CRIU-backed container checkpointing with archive export/import for restoring prepared emulator environments on compatible Linux hosts.
- [Serverless Devs](https://github.com/Serverless-Devs/Serverless-Devs) - Open-source serverless development CLI with Function Compute components for locally invoking and debugging Alibaba Cloud functions; it does not emulate dependent cloud services.
- [Testcontainers Azure Module](https://java.testcontainers.org/modules/azure/) - Java test integrations that manage Azurite, Event Hubs, Service Bus, and Cosmos DB emulator containers.
- [Testcontainers Google Cloud Module](https://java.testcontainers.org/modules/gcloud) - Java test integrations that manage the lifecycle of Google Cloud emulator containers.
- [Testcontainers LocalStack Module](https://java.testcontainers.org/modules/localstack/) - Java test integration that manages a LocalStack container and its endpoints; emulator features depend on the selected LocalStack plan.
- [Toxiproxy](https://github.com/Shopify/toxiproxy) - TCP proxy for injecting latency, connection failures, and other network faults between an application and a local emulator.

## Choosing an Emulator

Start with the operations and failure paths your tests need. Use mocks for isolated logic, service emulators for API integration, and broader suites when the interactions between services matter. Infrastructure API simulation does not necessarily provision a working database, VM, network, or function runtime.

Compare candidates using the [selection guide](docs/selection-guide.md#start-with-your-test-scenario). Verify your SDK or IaC provider version, endpoints, resource lifecycle, and cleanup behavior against a controlled cloud environment.

## Contributing

Suggestions and corrections are welcome. Read the [contribution guidelines](CONTRIBUTING.md) and [code of conduct](CODE_OF_CONDUCT.md), then open a pull request or use an issue template. Include upstream evidence, important limitations, and any affiliation.

## Support

If this list helps your work, you can [buy me a coffee](https://buymeacoffee.com/upgundecha) to support its maintenance.
