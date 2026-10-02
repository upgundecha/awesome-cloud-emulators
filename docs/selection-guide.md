# Selecting a cloud emulator

[Back to the list](../README.md)

Choose against a representative test, not a service count, implementation language, or a claim of full compatibility. This guide combines the catalog's service grouping with practical runtime, ownership, and validation questions.

**License markers:** 💰 Paid commercial plan or license for commercial use; a free tier or exception may exist. 📜 Vendor-specific software terms or an EULA apply; this does **not** mean a fee is required. Open-source licenses still apply to unmarked tools. Markers highlight verified conditions, not an exhaustive license audit.

## Contents

- [Start with your test scenario](#start-with-your-test-scenario)
- [Comparison by cloud](#comparison-by-cloud)
- [License and access notes](#license-and-access-notes)
- [Important distinctions](#important-distinctions)
- [Beyond emulation](#beyond-emulation)
- [Evaluation checklist](#evaluation-checklist)
- [Reusable emulator baselines](#reusable-emulator-baselines)
- [Adoption sequence](#adoption-sequence)
- [Evidence and reconciliation notes](#evidence-and-reconciliation-notes)

## Start with your test scenario

These are candidates to evaluate, not a benchmark ranking or a claim of interchangeable behavior.

| Scenario | Candidates | First validation |
| --- | --- | --- |
| Isolate Python logic using AWS SDK calls | Moto | Supported calls, mock lifecycle, and state isolation. |
| Exercise an AWS event-driven application | fakecloud, Floci, Hiraeth, LocalEmu, MiniStack, LocalStack | A complete publish → consume → retry path, not just resource creation. |
| Test one AWS dependency | ElasticMQ, S3Mock, S3Proxy; DynamoDB Local | Exact database, queue, or object operations required by the application. |
| Test an Azure application dependency | Azure Key Vault Emulator, Azurite; Cosmos DB, Event Hubs, or Service Bus emulators | SDK connectivity, TLS, and required data-plane behavior. |
| Test several Azure APIs together | Floci AZ, miniblue, Topaz; LocalStack for Azure | Cross-service behavior, runtime requirements, and which services need external engines or backends. |
| Test a Firebase application | Firebase Local Emulator Suite | Auth, Security Rules, database events, and function triggers used by the app. |
| Test Google Cloud data and messaging clients | Provider emulators; Fake GCS Server, Fullstory, BigQuery, Cloud Tasks, or Pub/Sub pstest for Go | Client endpoints, SQL/queries, messages, tasks, and failure semantics. |
| Test several Google Cloud APIs together | Floci GCP | REST/gRPC coverage and integrated behavior for your specific flow. |
| Test OCI resource and application integrations | Floci OCI | Endpoint routing, work requests, tenancy isolation, and required sidecars; signature parsing does not prove authorization. |
| Test Cloudflare Workers and bindings | Miniflare through Wrangler or its programmatic API | Local binding behavior, persistence, and differences from production; distinguish local execution from remote bindings. |
| Test European cloud infrastructure automation | Feint | Provider-specific CLI/IaC operation coverage, endpoint guards, and control-plane versus optional machine runtime behavior. |
| Test Snowflake SQL and data pipelines locally | LocalStack for Snowflake | Required SQL features, client connectivity, pipeline behavior, and Snowflake license access. |
| Test real VM boot and guest networking through AWS-compatible APIs | Spinifex | KVM host setup, cloud-init, SSH reachability, network rules, teardown, and differences from AWS. |
| Test infrastructure automation across clouds | cloudemu; Vera for EC2/Compute scope | Full create/read/update/delete lifecycle with the real CLI or IaC provider. |
| Run emulators in automated Java tests | Testcontainers Azure, Google Cloud, or LocalStack modules | Image readiness, fixture isolation, cleanup, and parallel execution. |
| Invoke and debug Lambda locally | AWS SAM CLI; Lambda Runtime Interface Emulator for container images | Runtime and event handling; configure external service dependencies separately. |
| Invoke and debug Alibaba Cloud functions locally | Serverless Devs with the appropriate FC component | Runtime and component version, local invocation, and separately configured service dependencies. |
| Run Azure Functions locally | Azure Functions Core Tools | Trigger behavior and connections to local service emulators. |
| Test network failures against an emulator | Toxiproxy | Verify retry, timeout, and recovery behavior without claiming cloud network parity. |
| Rebuild a baseline from recorded requests | Emulator recording or a client-side request log, such as Moto Recorder | Replay into a clean instance and verify generated identifiers and dependencies. |
| Resume a prepared emulator process | CRIU, Podman Checkpoint/Restore, Docker Checkpoint/Restore, DMTCP | Restore on the target runner and verify memory, external state, and clean test isolation. |

## Comparison by cloud

Every README entry appears below once. Tool links lead to primary documentation. **AWS, Microsoft, Google, and Cloudflare** in the maintainer column mean cloud-provider supplied; **community** includes independent individuals and company-sponsored open-source projects; **vendor** identifies a commercial platform. These labels do not determine quality or licensing.

Within each section, open-source tools come first, followed by other distributions, then paid/commercial tools; entries are alphabetized within each group. “Other distributions” includes free vendor binaries, mixed suites, and tools whose runtime source license has not been established. A usable open-source edition stays in the first group even when optional paid editions exist.

Delivery describes how you consume the tool, rather than guessing the implementation language of closed-source runtimes. Runtime versions and CPU architecture support should be checked for the exact release you intend to use.

### AWS

| Tool | Access group | API scope | How it runs | Maintainer | Key evaluation question |
| --- | --- | --- | --- | --- | --- |
| [ElasticMQ](https://github.com/softwaremill/elasticmq) | Open source | SQS-compatible queues | JVM server, embedded library, or Docker | Community / SoftwareMill | Verify FIFO, visibility timeouts, redelivery, and dead-letter behavior. |
| [fakecloud](https://fakecloud.dev) | Open source | AWS APIs | Binary or Docker; test SDKs | Community | Exercise async control and assertions; verify the operations you need. |
| [Floci](https://floci.io/floci) | Open source | AWS APIs | Docker / Compose | Community | Check service-to-service flows and persistence for your workload. |
| [Hiraeth](https://github.com/SethPyle376/hiraeth) | Open source | SQS; partial SNS/IAM; STS GetCallerIdentity | Rust binary or Docker / Compose; SQLite state | Community | Verify SNS-to-SQS delivery, FIFO gaps, IaC lifecycle, and audit versus enforce authorization. |
| [LocalEmu](https://localemu.cloud) | Open source | AWS APIs | Python CLI; Docker for selected engines | Community | Separate in-process API behavior from engine-backed requirements. |
| [MiniStack](https://ministack.org) | Open source | AWS APIs | Docker; selected external engines | Community | Check account/region isolation and which data planes actually execute. |
| [Moto](https://github.com/getmoto/moto) | Open source | AWS API mocks | Python library, server, or Docker | Community | Choose library or server mode; validate supported operations and behaviors. |
| [S3Mock](https://github.com/adobe/S3Mock) | Open source | S3 API subset | Docker / Testcontainers; JVM integrations | Community / Adobe | Check supported object operations and your chosen integration version. |
| [S3Proxy](https://github.com/gaul/s3proxy) | Open source | S3-compatible API over storage backends | Java server or Docker | Community | Choose a local backend and test the exact S3 operations your SDK uses. |
| [DynamoDB Local 📜](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.html) | Other distributions | DynamoDB | Java archive, Maven dependency, or Docker | AWS | Compare transactions, indexes, and cloud-only behavior. |
| [LocalStack 💰 📜](https://docs.localstack.cloud/aws) | Paid/commercial | AWS APIs | Container, managed through CLI or Docker | Vendor | Plan coverage, authentication, CI entitlements, and network requirements. |

### Microsoft Azure

| Tool | Access group | API scope | How it runs | Maintainer | Key evaluation question |
| --- | --- | --- | --- | --- | --- |
| [Azure Key Vault Emulator](https://github.com/james-gould/azure-keyvault-emulator) | Open source | Key Vault APIs | Docker; .NET Aspire integration | Community | Check secrets/keys/certificates coverage, TLS trust, and persistence. |
| [Azurite](https://github.com/Azure/Azurite) | Open source | Blob, Queue, Table Storage | npm CLI, Docker, or VS Code extension | Microsoft | Verify required API versions and storage-specific behavior. |
| [Floci AZ](https://github.com/floci-io/floci-az) | Open source | Azure APIs | Docker / Compose; engine-backed services | Community | Verify SDK routing, TLS, auth mode, and Docker requirements per service. |
| [miniblue](https://github.com/moabukar/miniblue) | Open source | Multiple Azure APIs | Go binary, Homebrew, or Docker | Community | Verify supported operations, certificate trust, and which services use real backends. |
| [Topaz](https://github.com/TheCloudTheory/Topaz) | Open source | Azure control and data plane APIs | Single binary, Homebrew, or Docker | Community | Verify template deployments, RBAC and identity behavior, and per-service coverage. |
| [Azure Cosmos DB Emulator](https://learn.microsoft.com/en-us/azure/cosmos-db/emulator) | Other distributions | Cosmos DB | Windows or container variants | Microsoft | Choose the correct API, OS, and architecture variant; check TLS setup. |
| [Azure Event Hubs Emulator 📜](https://learn.microsoft.com/en-us/azure/event-hubs/overview-emulator) | Other distributions | Event Hubs | Container with Azurite dependency | Microsoft | Check protocols, partition behavior, limits, and restart persistence. |
| [Azure Service Bus Emulator 📜](https://learn.microsoft.com/en-us/azure/service-bus-messaging/overview-emulator) | Other distributions | Service Bus | Container with database dependency | Microsoft | Check messaging features, dependencies, limits, and state reset. |
| [LocalStack for Azure 💰 📜](https://docs.localstack.cloud/azure) | Paid/commercial | Azure Resource Manager and selected data plane APIs | Container, managed through CLI or Docker | Vendor | Preview access, CI Auth Tokens, per-service coverage, and network requirements. |

### Google Cloud and Firebase

| Tool | Access group | API scope | How it runs | Maintainer | Key evaluation question |
| --- | --- | --- | --- | --- | --- |
| [BigQuery Emulator](https://github.com/goccy/bigquery-emulator) | Open source | BigQuery APIs | Go server or Docker | Community | Test required SQL dialect, jobs, types, and storage APIs against real BigQuery. |
| [Bigtable Emulator](https://cloud.google.com/bigtable/docs/emulator) | Open source | Bigtable | gcloud component; container workflows | Google | Check mutations and filters; do not infer production replication behavior. |
| [Cloud Tasks Emulator](https://github.com/aertje/cloud-tasks-emulator) | Open source | Cloud Tasks | Go server or Docker | Community | Test dispatch, retry, scheduling, and authentication behavior explicitly. |
| [Fake GCS Server](https://github.com/fsouza/fake-gcs-server) | Open source | Cloud Storage | Go library, binary, or Docker | Community | Check signed URLs, endpoint configuration, and supported operations. |
| [Floci GCP](https://floci.io/floci-gcp) | Open source | Google Cloud APIs | Docker / Compose; selected engine containers | Community | Check REST/gRPC coverage and service-specific credential behavior. |
| [Fullstory Emulators](https://github.com/fullstorydev/emulators) | Open source | Bigtable and Cloud Storage | Go libraries or standalone servers | Community / Fullstory | Check client setup and persistent storage; this is a collection, not one shared endpoint. |
| [Pub/Sub pstest](https://pkg.go.dev/cloud.google.com/go/pubsub/v2/pstest) | Open source | Fake Pub/Sub API for Go tests | In-process Go gRPC server | Google Cloud Go client library | Check supported calls and behavior; distinct from the standalone Pub/Sub Emulator. |
| [Spanner Emulator](https://cloud.google.com/spanner/docs/emulator) | Open source | Spanner | gcloud component or Docker | Google | Check SQL dialect, transactions, and documented operational differences. |
| [Datastore Emulator](https://cloud.google.com/datastore/docs/tools/datastore-emulator) | Other distributions | Datastore APIs | gcloud component with Java runtime | Google | Check database mode, queries, indexes, and persistence settings. |
| [Firebase Local Emulator Suite](https://firebase.google.com/docs/emulator-suite) | Other distributions | Firebase products and overlapping GCP APIs | Firebase CLI with service-specific runtimes | Google / Firebase | Check enabled products, Security Rules, triggers, and fixture import/export. |
| [Firestore Emulator](https://cloud.google.com/firestore/native/docs/emulator) | Other distributions | Firestore | gcloud or Firebase CLI with Java runtime | Google | Check transactions, queries, and differences from production indexes and limits. |
| [Pub/Sub Emulator](https://cloud.google.com/pubsub/docs/emulator) | Other distributions | Pub/Sub | gcloud component with Java runtime; container workflows | Google | Check acknowledgments, ordering, retry, and subscription features. |

### Oracle Cloud

| Tool | Access group | API scope | How it runs | Maintainer | Key evaluation question |
| --- | --- | --- | --- | --- | --- |
| [Floci OCI](https://github.com/floci-io/floci-oci) | Open source | OCI identity, object storage, messaging, and selected other APIs | Docker / Compose or Java; optional Fn Project and k3s sidecars | Community | Verify supported operations, tenancy/persistence behavior, and sidecars; signatures are parsed but not verified. |

### Cloudflare

| Tool | Access group | API scope | How it runs | Maintainer | Key evaluation question |
| --- | --- | --- | --- | --- | --- |
| [Miniflare](https://developers.cloudflare.com/workers/testing/miniflare/) | Open source | Workers execution and local service bindings | Node.js library or Wrangler local development, using workerd | Cloudflare | Check binding coverage and persistence; local tests do not establish edge-network behavior or full management API compatibility. |

### Cross-Cloud

| Tool | Access group | API scope | How it runs | Maintainer | Key evaluation question |
| --- | --- | --- | --- | --- | --- |
| [cloudemu](https://github.com/stackshy/cloudemu) | Open source | AWS, Azure, GCP API simulation | Server, Docker, or embedded Go | Community | In-memory API state is not provisioned infrastructure; test reset and endpoint routing. |
| [Feint](https://github.com/stephrobert/feint) | Open source | Scaleway, Outscale, and Exoscale APIs | Go binary or container; optional Incus/OVN machine runtime | Community | Validate provider-specific CLI/IaC lifecycle and endpoint routing; authentication, quotas, capacity, and eventual consistency differ. |
| [Vera](https://github.com/project-vera/vera) | Other distributions | AWS EC2 and Google Compute APIs | Docker Compose | Community | Scope is compute/infrastructure simulation, not all AWS or GCP services. |

### Snowflake

| Tool | Access group | API scope | How it runs | Maintainer | Key evaluation question |
| --- | --- | --- | --- | --- | --- |
| [LocalStack for Snowflake 💰 📜](https://docs.localstack.cloud/snowflake) | Paid/commercial | Snowflake SQL APIs | Container, managed through CLI or Docker | Vendor | SQL feature coverage, SDK and pipeline integrations, and license activation. |

### Supporting Tools

| Tool | Access group | API scope | How it runs | Maintainer | Key evaluation question |
| --- | --- | --- | --- | --- | --- |
| [AWS Lambda Runtime Interface Emulator](https://github.com/aws/aws-lambda-runtime-interface-emulator) | Open source | Lambda runtime invocation for container images | Lightweight local proxy / container runtime | AWS | Test function invocation; it does not reproduce Lambda's full managed environment. |
| [AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/using-sam-cli-local-testing.html) | Open source | Local serverless execution | CLI with local/container runtimes | AWS | Supporting tool; supply emulators for service dependencies separately. |
| [Azure Functions Core Tools](https://learn.microsoft.com/en-us/azure/azure-functions/functions-run-local) | Open source | Local Functions execution | CLI with local Functions host | Microsoft | Check trigger and binding dependencies; connect to separate local emulators as needed. |
| [CRIU](https://criu.org) | Open source | Linux process state | Linux checkpoint/restore utility | CRIU project | Verify kernel capabilities, privileges, sockets, and the target host; no emulator-specific certification implied. |
| [DMTCP](https://github.com/dmtcp/dmtcp) | Open source | Application process state | Linux launcher and coordinator | DMTCP project | Validate the Python process, libraries, threads, and external connections; not a drop-in OCI snapshot. |
| [Docker Checkpoint/Restore](https://docs.docker.com/reference/cli/docker/checkpoint) | Open source | Container process state | Docker Engine with CRIU | Docker / Moby | Experimental; verify daemon/runtime support and restore behavior on the actual Linux host. |
| [Moto Recorder](https://docs.getmoto.org/en/stable/docs/configuration/recorder/index.html) | Open source | Recorded Moto requests | Moto feature; ServerMode APIs | Moto project | Enable recording; replay into clean state; generated identifiers need explicit validation. |
| [Podman Checkpoint/Restore](https://podman.io/docs/checkpoint) | Open source | Container process state | Podman with CRIU and compatible OCI runtime | Podman project | Verify export/import, host compatibility, privileges, mounted data, and network identity. |
| [Serverless Devs](https://github.com/Serverless-Devs/Serverless-Devs) | Open source | Local Alibaba Cloud Function Compute invocation/debugging through FC components | Node.js CLI with component-specific local runtime/container requirements | Community / Serverless Devs | Choose the correct FC component and runtime; distinguish local calls from deployment/remote invocation and provide service dependencies separately. |
| [Testcontainers Azure Module](https://java.testcontainers.org/modules/azure/) | Open source | Azure emulator lifecycle in Java tests | Java test library controlling containers | Testcontainers project | Check image prerequisites, certificates, readiness, and isolated test state. |
| [Testcontainers Google Cloud Module](https://java.testcontainers.org/modules/gcloud) | Open source | Emulator lifecycle in Java tests | Java test library controlling containers | Testcontainers project | Supporting tool; fidelity and licensing come from the selected emulator image. |
| [Testcontainers LocalStack Module](https://java.testcontainers.org/modules/localstack/) | Open source | LocalStack lifecycle in Java tests | Java test library controlling a container | Testcontainers project | Check LocalStack image version, plan access, endpoints, and supported services. |
| [Toxiproxy](https://github.com/Shopify/toxiproxy) | Open source | TCP network fault injection | Proxy server with test clients | Community / Shopify | Route emulator traffic through it and verify retry or timeout behavior; not a cloud emulator. |

## License and access notes

Existing entries checked against primary sources on **2026-09-28**; Spinifex and LocalStack for Azure checked on **2026-09-29**; LocalStack for Snowflake checked on **2026-09-30**. These markers describe the listed distribution; dependencies and optional hosted services can carry separate terms.

| Tool | Markers | What to check | Primary source |
| --- | --- | --- | --- |
| Spinifex | 💰 | AGPL-3.0 is free; an optional paid commercial edition includes a separate license, enterprise features, and support. | [Purchase and editions](https://mulgadc.com/purchase), [source license](https://github.com/mulgadc/spinifex/blob/main/LICENSE). |
| LocalStack | 💰 📜 | Commercial use is offered through paid plans, subject to vendor exceptions/programs. Hobby is free for non-commercial use; activation requires an account/token. | [Plans and pricing](https://www.localstack.cloud/pricing), [activation](https://docs.localstack.cloud/aws/getting-started/installation). |
| LocalStack for Azure | 💰 📜 | Private preview: Azure access is enabled on request for an active LocalStack subscription (paid, trial, or Hobby). Commercial use is offered through paid plans; Hobby is free for non-commercial use. Activation requires an auth token; CI requires a CI Auth Token. | [Preview access and activation](https://docs.localstack.cloud/azure/getting-started/auth-token/#managing-your-license), [plans and pricing](https://www.localstack.cloud/pricing), [Terms of Service](https://www.localstack.cloud/legal/terms-of-service). |
| LocalStack for Snowflake | 💰 📜 | Snowflake access is available through a trial or paid offering. Activation requires an auth token and an assigned Snowflake license. | [License access and activation](https://docs.localstack.cloud/snowflake/getting-started/auth-token/), [Terms of Service](https://www.localstack.cloud/legal/terms-of-service). |
| DynamoDB Local | 📜 | The downloadable software has a specific AWS license agreement. This marker identifies those conditions, not a paid emulator subscription. | [DynamoDB Local License Agreement](https://aws.amazon.com/dynamodb/dynamodblocallicense). |
| Azure Event Hubs Emulator | 📜 | Startup requires accepting Microsoft's software terms through `ACCEPT_EULA`. | [Microsoft setup and EULA instructions](https://learn.microsoft.com/en-us/azure/event-hubs/test-locally-with-event-hub-emulator). |
| Azure Service Bus Emulator | 📜 | Setup requires accepting the emulator and SQL Server Linux terms through `ACCEPT_EULA`. | [Microsoft setup and EULA instructions](https://learn.microsoft.com/en-us/azure/service-bus-messaging/test-locally-with-service-bus-emulator). |

The access groups were reviewed on **2026-10-01**. Open-source classification refers to the listed tool or integration, not every dependency: Testcontainers modules remain open source even when they manage paid images, and Docker Checkpoint/Restore refers to Docker Engine/Moby rather than Docker Desktop. Firebase combines an open-source CLI with separately downloaded emulator runtimes. Vera's public repository did not establish a runtime license during this review, so it remains under Other distributions. [Bigtable's emulator source](https://github.com/googleapis/google-cloud-go/tree/main/bigtable/cmd/emulator) and [Spanner's source](https://github.com/GoogleCloudPlatform/cloud-spanner-emulator) are available under Apache-2.0.

An unmarked entry is not a claim of unrestricted use or a verified open-source runtime. For example, the MIT license in the Cosmos DB emulator's supporting GitHub repository should not be used to infer the license of every downloadable runtime; review the terms shipped with the selected image or installer. Apply the same release-specific check to SDK-bundled components and container dependencies.

## Important distinctions

- **Mocks, API emulators, and real engines:** a mocked `CreateDBInstance` response is different from a reachable database. Some suites start real database or function containers for selected services. Test both control-plane lifecycle and data-plane use where relevant.
- **Official does not mean complete:** provider emulators also have limits. Local results cannot establish production IAM, network isolation, performance, availability, or multi-region behavior.
- **SDK compatibility is not semantic parity:** accepting a request does not prove ordering, authorization, consistency, or error behavior. Test negative paths and retry handling.
- **Suite versus collection:** Firebase integrates multiple local products; Fullstory ships separate Bigtable and Storage implementations. Do not assume a shared endpoint or integrated events because tools occupy the same category.
- **Language versus runtime:** Go code may be used through a server by any SDK; Java may be a runtime prerequisite rather than a verified implementation language. Compare setup and integration requirements first.
- **Local does not necessarily mean offline or free:** downloads, activation, telemetry, feature tiers, commercial-use conditions, and CI terms vary. Inspect the chosen release and plan.

### LocalStack source and product status

The [former LocalStack source repository](https://github.com/localstack/localstack) is archived, but its notice directs users to the unified product. The catalog therefore links current product documentation rather than treating an archived checkout as the supported distribution. Current [installation documentation](https://docs.localstack.cloud/aws/getting-started/installation) requires authentication to activate AWS features; check plan and CI conditions before adopting it.

LocalStack for Azure has no public source repository, so the catalog links its [documentation](https://docs.localstack.cloud/azure). It is in private preview; review the [license and access notes](#license-and-access-notes) before adopting it.

LocalStack for Snowflake runs in a single container for local SQL and data pipeline testing without a real Snowflake account. Its documentation includes [feature coverage](https://docs.localstack.cloud/snowflake/feature-coverage/) and [SQL functions](https://docs.localstack.cloud/snowflake/sql-functions/) references; see the [license and access notes](#license-and-access-notes) for activation requirements.

### Hiraeth coverage

[Hiraeth](https://github.com/SethPyle376/hiraeth) is an early MIT-licensed project focused on SQS, with partial SNS and IAM APIs and STS `GetCallerIdentity`. Its [SQS notes](https://github.com/SethPyle376/hiraeth/blob/main/docs/sqs/README.md) document incomplete FIFO ordering/deduplication and unimplemented redrive. Its [SNS notes](https://github.com/SethPyle376/hiraeth/blob/main/docs/sns/README.md) limit subscriptions to SQS and exclude message filtering. The [IAM notes](https://github.com/SethPyle376/hiraeth/blob/main/docs/iam/README.md) distinguish the default audit mode, which logs authorization decisions but allows requests, from enforce mode; policy conditions and other IAM semantics are incomplete. Validate the exact Terraform or SDK lifecycle you need rather than assuming broad IAM support.

### Fullstory coverage

Fullstory's repository ships Bigtable and Cloud Storage implementations. Its README also discusses Google's `pubsub/pstest`, now listed separately as an in-process Go fake. The provider's standalone Pub/Sub Emulator remains a distinct entry.

### Additional provider coverage

[Floci OCI's service notes](https://github.com/floci-io/floci-oci#supported-services) distinguish in-process APIs from optional Fn Project and k3s execution. Request signatures supply tenancy/user context but are not verified, and IAM policy statements are stored rather than proof of real OCI authorization. Check operation-level gaps, including object versioning and lifecycle policies.

[Feint's limits](https://github.com/stephrobert/feint/blob/main/docs/limits.md) and [confidence guide](https://github.com/stephrobert/feint/blob/main/docs/confidence.md) separate API simulation from optional Incus-backed execution. Real machine/network tests require compatible Linux runtime infrastructure; the control-plane container does not supply it. Guard provider-specific endpoints before IaC runs, and do not infer cloud authentication, quota, capacity, or eventual-consistency behavior.

[Miniflare](https://developers.cloudflare.com/workers/testing/miniflare/) lives in the current [Workers SDK repository](https://github.com/cloudflare/workers-sdk/tree/main/packages/miniflare), replacing the former standalone repository. It simulates Workers and bindings rather than Cloudflare's entire management API. [Wrangler local development](https://developers.cloudflare.com/workers/local-development/) can be combined with remote bindings; check configuration before assuming every dependency is local or offline.

[Serverless Devs local invocation](https://help.aliyun.com/en/functioncompute/faq-about-serverless-devs-1) is a supporting capability delivered through Function Compute components. Select the component for the target FC generation and validate runtime/debugger limitations. Local function execution does not reproduce OSS, queues, networking, or authorization in the cloud.

## Beyond emulation

A self-hosted AWS-compatible platform can run real workloads when a mock API response cannot validate VM boot, cloud-init, SSH, or guest networking. This is a different deployment choice from a local emulator: budget for virtualization-capable Linux hosts, network setup, storage, and cleanup. Continue to validate AWS-specific behavior in a controlled AWS account.

| Platform | Scope | How it runs | First validation |
| --- | --- | --- | --- |
| [Spinifex 💰](https://github.com/mulgadc/spinifex) | AWS-compatible infrastructure APIs and real workload execution | Self-hosted Linux platform using QEMU/KVM, block and object storage, and OVN networking | Verify needed API operations, guest boot and connectivity, host prerequisites, and differences from AWS. |

The [AGPL-3.0 edition](https://github.com/mulgadc/spinifex/blob/main/LICENSE) is free; [a paid commercial edition](https://mulgadc.com/purchase) offers another licensing option and support. The 💰 marker denotes that paid option, not a requirement to pay for the open-source edition.

## Evaluation checklist

| Dimension | Questions to answer | Evidence to capture |
| --- | --- | --- |
| API coverage | Are required operations, SDK versions, protocols, and errors supported? | Operation checklist tied to application tests. |
| Behavior | Do transactions, ordering, retries, timeouts, and consistency meet test assumptions? | Positive and negative test results against emulator and cloud. |
| Identity and networking | Are permissions enforced, bypassed, or partially modeled? Is TLS required? | Auth mode, trust configuration, and real-cloud test gaps. |
| Lifecycle | Can tests seed, reset, and destroy state? Is persistence intentional? | Repeatable fixtures and a clean teardown run. |
| Runtime | Are OS, architecture, dependencies, and container privileges compatible with CI? | Successful run on the actual CI runner class. |
| Connectivity | Do installation, startup, licensing, or service execution require network access? | Observed egress and offline-start result if needed. |
| Access and terms | Are accounts, tokens, EULA acceptance, paid features, or commercial/CI terms relevant? | Links to the selected release's license and terms. |
| Maintenance | Are releases, documentation, and issue handling sufficient for your use? | Dated review of upstream evidence; no undated “active” badge. |
| Isolation | Can parallel jobs avoid shared-state collisions or real-cloud calls? | Unique endpoints/resources, dummy credentials, and endpoint guards. |
| Operational support | Can failures be diagnosed and upgrades rolled back? | Logs, version/image pin, and upgrade comparison results. |
| Production validation | Which properties are outside the emulator's model? | A named real-cloud suite and an owner for each gap. |

## Reusable emulator baselines

A baseline can capture a modeled landing-zone subset: supported account or project setup, storage, messaging, databases, policies, and test data. It does not establish real-cloud policy enforcement or landing-zone compliance.

| Strategy | What is reused | Suitable starting point | Important boundary |
| --- | --- | --- | --- |
| Declarative setup or seed code | Instructions to recreate resources | SDK/IaC baseline targeting local endpoints | Re-run setup; retain versioned fixture code and account for unsupported APIs. |
| Emulator-native export or persistence | State in an emulator-supported format or data directory | Firebase export/import, LocalStack snapshots, Azurite persistence, DynamoDB Local database | Check feature coverage, version compatibility, licensing, and worker isolation. |
| Request replay | Previously recorded API calls | Moto Recorder | Recreates state; does not serialize process memory or automatically preserve generated IDs. |
| Process/container checkpoint | Captured execution state | CRIU, Podman, experimental Docker checkpointing; DMTCP for suitable processes | Environment-dependent restore; external services and mounted files need separate consistency handling. |
| Filesystem or volume copy | Persisted files | Emulators with documented disk-backed state | A disk copy alone cannot restore in-memory state; stop or quiesce writes before capture. |

Start with reproducible setup. Consider native export or persistence when the baseline is reused across runs or teams, workers need isolated copies, or setup is costly; then consider request replay and consistent disk copies. Trial process checkpoints on compatible Linux hosts only when simpler methods do not meet the need. Compare rebuild and restore time, reliability, artifact size, and the work to regenerate artifacts after baseline changes. Keep a rebuild path when artifacts become incompatible.

These combinations are research candidates; verify restore behavior and isolation on the intended runner before relying on them.

## Adoption sequence

1. **Define a small vertical slice.** Record the SDK/IaC version, operations, event flow, expected errors, and acceptable differences.
2. **Shortlist the smallest useful tool.** A single-service emulator may be enough; select a suite when interactions matter.
3. **Run the same scenario locally and in a controlled cloud account.** Include a negative case, retry path, restart, and cleanup. For IaC, include refresh and destroy rather than plan alone.
4. **Record the result.** Use the decision record below. Pin the tool version or image digest and document gaps.
5. **Integrate in CI.** Wait for readiness, isolate jobs, reset state, capture logs, and confirm cleanup after failure.
6. **Keep cloud validation.** Retain tests for permissions, networking, deployment, performance, and resilience properties that local emulation cannot establish.
7. **Review on upgrades.** Re-run the comparison when the emulator, SDK, provider plugin, or required service behavior changes.

### Minimal decision record

| Field | Record |
| --- | --- |
| Candidate and version | Tool, release or image digest, and primary documentation. |
| Scenario and client | Required services, operations, SDK/IaC version, and test entry point. |
| Results | Passed/failed cases, logs, runtime, and resource use measured on your runner. |
| Known gaps | Semantic differences and features intentionally not modeled. |
| Access and setup | License/plan, activation, dependencies, endpoint and state configuration. |
| Cloud backstop | Tests that remain in a controlled real-cloud environment. |
| Decision | Adopt, trial, or reject; owner and review date. |

## Evidence and reconciliation notes

Documentation review: **2026-09-28**; Hiraeth and access-group ordering reviewed on **2026-10-01**. Additional provider entries reviewed on **2026-10-02**. This is a documentation-based comparison, not a hands-on compatibility certification or performance benchmark.

- Reconciled the original repository catalog with `awesome-cloud-emulators-README-v2.txt`: retained all 18 existing entries, added 12 distinct tools, and merged naming/URL variants rather than duplicating them. That reconciliation produced 28 emulator/mock entries plus 2 supporting tools. A subsequent checkpoint/baseline review added 5 supporting tools or features. This review added 2 focused API test doubles and 5 supporting tools, bringing the emulator and supporting-tool catalog to 42 entries: 30 emulators/mocks and 12 supporting entries. Spinifex was subsequently added in the separate Beyond Emulation section and is outside that count.
- Preserved the attachment's provider and multi-service/single-service organization, while keeping detailed comparisons in this guide and a concise catalog in the README.
- Used upstream documentation linked in the tables. Removed exact service counts, benchmark claims, blanket “active” labels, and hard-coded Java versions that do not establish suitability and can quickly become stale.
- Removed the attachment's Cloud Tasks inactivity claim: the [repository metadata](https://api.github.com/repos/aertje/cloud-tasks-emulator) reported a push on 2026-09-09 during this review. A recent push alone does not establish maintenance quality or compatibility.
- Narrowed Fullstory's shipped coverage to Bigtable and Cloud Storage, and Vera's scope to EC2 and Google Compute. Distinguished LocalStack's archived source from its current product.
- Treated the attachment's Hacker News link as discovery context, not technical evidence or an independently verified origin claim for this repository.
- The 2026-10-02 provider review added Feint, Floci OCI, Miniflare, and Serverless Devs after checking 50+ repository stars, non-archived status, and repository pushes in 2026. Miniflare shares the Workers SDK repository's star count; Serverless Devs' FC components have separate repositories and counts. These discovery filters do not establish maintenance quality or compatibility. Feint, Floci OCI, and Miniflare are Apache-2.0, MIT, and MIT respectively; Serverless Devs is Apache-2.0, while its FC3 component is MIT. The current catalog contains 52 entries, including Beyond Emulation.
- Preserved the existing CC0 license and contribution policy; omitted the attachment's obsolete instruction to add a license file.
