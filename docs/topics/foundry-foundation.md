# Microsoft Foundry foundation

Verified: **July 29, 2026**

> This document was prepared by a gap-research session for use during a later study session. The questions and exit check are stored here; they are not administered during research.

## Official exam relationship

This is a cross-cutting foundation for the AI-901 implementation domain. It supports the official objectives that require you to:

- Select model deployment options and configuration parameters.
- Deploy a model and interact with it in the Microsoft Foundry portal.
- Build lightweight AI applications with Foundry services, endpoints, REST APIs, and SDKs.

It is not a separate exam domain.

Study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901

## Handwritten-friendly summary

- A **tenant** is the Microsoft Entra identity boundary.
- A **subscription** is a billing, quota, access, and governance boundary.
- A **resource group** groups Azure resources that are managed together.
- A **Foundry resource** is the shared Azure platform boundary for security, governance, monitoring, quotas, model deployments, and projects.
- A **Foundry project** is a workspace inside a Foundry resource for one application or use case.
- A **model deployment** makes a selected model version callable with a deployment name and configuration.
- An **endpoint** is the URL a portal, REST request, or SDK client calls.
- A **credential** proves the caller's identity. An **RBAC role** determines what that identity may do.

Core flow:

`tenant → subscription → resource group → Foundry resource → project → deployment or tool → endpoint + credential → client request`

## Azure and Foundry hierarchy

| Level | Main responsibility | Remember |
|---|---|---|
| Microsoft Entra tenant | Users, groups, applications, service principals, and managed identities | Identity boundary |
| Azure subscription | Billing, quotas, policies, and broad access scope | Resource and cost boundary |
| Resource group | Lifecycle and organization of related Azure resources | Delete the group and its resources are deleted |
| Foundry resource | Shared security, networking, monitoring, quotas, model deployments, and project container | Administrative and platform boundary |
| Foundry project | Assets and configuration for a team, app, or use case | Workspace inside a resource |
| Deployment or service asset | Callable model or configured capability | Runtime target |
| Client application | Sends requests and processes responses | Uses an endpoint and credential |

### Resource versus project versus deployment

| Component | Owns or organizes | Does not mean |
|---|---|---|
| Foundry resource | Shared governance, security, monitoring, quota, projects, and model hosting | One application workspace only |
| Foundry project | Agents, files, datasets, indexes, evaluations, connections, and app-specific settings | A separate subscription or resource quota |
| Model deployment | Model version plus capacity, filtering, rate, and deployment-name configuration | The model catalog entry itself |

A Foundry resource can contain multiple projects and multiple model deployments.

## Deployment mental model

A model in the catalog is a **model option**. A deployment is the **configured callable instance** of that model.

`catalog model + version + capacity/provisioning + filters + deployment name → callable deployment`

For Azure model calls, an SDK parameter named `model` often expects your **deployment name**.

Example:

```python
response = client.responses.create(
    model="support-gpt",  # Deployment name
    input="Summarize this support case.",
)
```

`support-gpt` might deploy a particular GPT model and version. The application addresses the deployment name so the underlying version or configuration can be managed independently.

Detailed model selection, capacity, and parameter practice belongs to the later generative-model gap session.

## Control plane and data plane

| Plane | Purpose | Examples |
|---|---|---|
| Control plane | Create and configure Azure and Foundry objects | Create a resource or project, assign roles, configure networking, create connections, deploy a model |
| Data plane | Use a deployed capability at runtime | Generate a response, call an agent, run an evaluation, analyze content, create embeddings |

Easy rule:

- **Control plane:** manage the platform.
- **Data plane:** use the platform.

The planes can have different permissions. Being allowed to create or view a resource does not automatically mean the identity can invoke every data-plane capability.

## Endpoint selection

There is no single URL for every Foundry operation. Select the endpoint that belongs to the operation and SDK.

| Need | Typical endpoint/client | Recognition clue |
|---|---|---|
| Work with Foundry project APIs and project-scoped capabilities | Project endpoint with the Foundry SDK | URL includes `/api/projects/<project-name>` |
| Use OpenAI-compatible model APIs directly | OpenAI-compatible `/openai/v1` endpoint | Best OpenAI API compatibility; request still uses a deployment name |
| Use a deployment-scoped legacy or API-versioned route | Resource endpoint plus `/deployments/<deployment-name>` | Deployment appears in the path |
| Use Azure Speech, Language, Content Understanding, or another Foundry Tool | The service's endpoint and SDK | Service-specific client and request model |

### Current project endpoint shape

```text
https://<resource-name>.services.ai.azure.com/api/projects/<project-name>
```

### Current OpenAI-compatible endpoint shape

```text
https://<resource-name>.openai.azure.com/openai/v1/
```

Do not memorize one endpoint as universal. Recognize the relationship:

`operation → matching SDK/client → matching endpoint → matching request`

## Authentication and authorization

These are two separate checks:

1. **Authentication:** Who or what is calling?
2. **Authorization:** What is that identity permitted to do at this scope?

| Failure | Usual meaning | Check first |
|---|---|---|
| `401 Unauthorized` | Credential, key, or token is missing, invalid, or expired | Endpoint, key, token acquisition, token scope |
| `403 Forbidden` | Identity is known but lacks the required permission | RBAC role and assignment scope |

A successfully authenticated user can still receive `403 Forbidden`.

## API key or Microsoft Entra ID?

| Option | Best fit | Strength | Limitation |
|---|---|---|---|
| API key | Quick isolated prototype or simple test | Easy to use | Static resource secret, coarse access, weak per-user auditability |
| Entra user identity | Interactive development | User-specific identity and RBAC | Requires sign-in and correct role assignments |
| Service principal | External automation or application identity | Explicit non-human identity | Secret or certificate lifecycle must be managed |
| Managed identity | Azure-hosted production application | No application secret to store | Available only to supported Azure resources and still needs RBAC |

Microsoft recommends **Microsoft Entra ID for production**. Use managed identity when the calling application runs in Azure and supports it.

### Secret safety

- Never place keys in source code or commit them to GitHub.
- Store unavoidable secrets in a secure secret store such as Azure Key Vault.
- Prefer `DefaultAzureCredential` so local development and Azure hosting can use appropriate Entra identities without changing application logic.

## Foundry RBAC recognition

Current role names include:

| Role | Recognition-level purpose |
|---|---|
| Foundry Agent Consumer | Interact with agent endpoints without broader development access |
| Foundry User | Build and test with existing projects and deployed capabilities |
| Foundry Project Manager | Manage project-level work and publish agents; can perform selected role assignment actions |
| Foundry Account Owner | Manage Foundry resources, projects, and models; high privilege |
| Foundry Owner | Broad management plus development/data-plane access; highly privileged |

Some pages or environments may still display the former names **Azure AI User**, **Azure AI Project Manager**, **Azure AI Account Owner**, and **Azure AI Owner** while the rename rolls out. The rename does not create a second set of concepts.

Role assignments always combine:

`identity + role + scope`

A correct role assigned at the wrong scope may not grant the required access.

## Minimal current Python project-client recognition

Current new-Foundry preparation uses `azure-ai-projects` **2.x**.

```python
from azure.ai.projects import AIProjectClient
from azure.identity import DefaultAzureCredential

project_client = AIProjectClient(
    endpoint=(
        "https://<resource-name>.services.ai.azure.com/"
        "api/projects/<project-name>"
    ),
    credential=DefaultAzureCredential(),
)
```

What to recognize:

- `AIProjectClient` works with Foundry project APIs.
- The endpoint identifies both the Foundry resource and project.
- `DefaultAzureCredential` obtains a Microsoft Entra identity from the available environment.
- Authentication is not enough; the selected identity also needs an appropriate RBAC role.

A project client can provide an OpenAI-compatible client for project-scoped model, Responses API, and agent operations:

```python
with project_client.get_openai_client() as openai_client:
    response = openai_client.responses.create(
        model="support-gpt",  # Deployment name
        input="Summarize this support case.",
    )
```

The full chat and agent client flow belongs to the later **Foundry SDK chat and agent clients** gap session.

## C# mental mapping

| Python | C# developer mental model |
|---|---|
| `DefaultAzureCredential()` | `new DefaultAzureCredential()` from `Azure.Identity` |
| `AIProjectClient(...)` | Constructing an Azure SDK client with endpoint and `TokenCredential` |
| `endpoint="..."` | A configured `Uri` or options value |
| `project_client.get_openai_client()` | Obtaining a specialized client from a higher-level project client |
| `responses.create(...)` | Calling an async SDK operation with a request object or named arguments |
| `model="support-gpt"` | Supplying the Azure deployment name |

Exam goal: read the flow and recognize the missing component. General Python mastery is unnecessary.

## Current-versus-classic freshness warning

| Guidance | New Foundry | Foundry classic |
|---|---|---|
| Python `azure-ai-projects` | 2.x | 1.x |
| .NET `Azure.AI.Projects` | 2.x family | 1.x family |
| Main orientation | Current project endpoint and current Foundry APIs | Older project types and samples |

Do not combine imports, client constructors, package versions, and endpoint formats from different generations merely because each fragment looks valid.

Also avoid new preparation based on the **Azure AI Inference beta SDK**. Microsoft has deprecated it and announced retirement for **August 26, 2026**. Prefer current Foundry SDK and GA OpenAI/v1 guidance.

## Prepared scenario practice

Use these questions during a future study or review session. Answer before opening the key.

1. A company needs separate workspaces for a support assistant and an invoice-analysis app, but wants shared governance, security, monitoring, and model quota. What should it create?
2. An administrator creates a Foundry resource, configures networking, assigns roles, and deploys a model. Are these control-plane or data-plane operations?
3. An application sends a prompt to a deployed model and receives a generated response. Is this a control-plane or data-plane operation?
4. A developer can sign in and open the project but receives `403 Forbidden` when calling an agent. What is the most likely category of problem?
5. An Azure App Service must call Foundry in production without storing a client secret. Which identity approach best fits?
6. A one-hour prototype in an isolated test subscription needs the simplest authentication. Which option is acceptable, and what production warning applies?
7. A request uses `model="claims-assistant"`, while the catalog model is a particular GPT version. What does `claims-assistant` most likely represent?
8. A URL contains `/api/projects/claims-project`. Which endpoint category is it?
9. A developer copies `azure-ai-projects==1.0.0` imports into a new-Foundry 2.x sample. What should be checked before troubleshooting the business logic?
10. A user is authenticated successfully but has no role assignment at the resource or project scope. Which HTTP category is more likely: `401` or `403`?

## Prepared code-recognition practice

11. Which class belongs in the first blank?

```python
from azure.ai.projects import __________
from azure.identity import DefaultAzureCredential
```

12. Which credential belongs in the second blank for Entra-based development?

```python
project_client = AIProjectClient(
    endpoint=project_endpoint,
    credential=____________________(),
)
```

13. What type of endpoint belongs in `project_endpoint`?

```text
https://<resource-name>.services.ai.azure.com/________________________
```

14. In an Azure model request, what should normally replace the blank?

```python
response = openai_client.responses.create(
    model="_____________",
    input="Classify the request.",
)
```

15. What does this method obtain?

```python
openai_client = project_client.get_openai_client()
```

- A new Azure subscription
- An OpenAI-compatible client associated with the project
- An API key from Key Vault

<details>
<summary><strong>Answer key</strong></summary>

1. **One Foundry resource with separate Foundry projects.** The resource provides the shared platform boundary; projects isolate app workspaces.
2. **Control plane.** They configure and manage platform resources.
3. **Data plane.** It invokes a runtime capability.
4. **Authorization/RBAC.** The identity is recognized, but it lacks permission for the operation or scope.
5. **Managed identity with an appropriate Foundry RBAC role.** It avoids storing an application secret.
6. **An API key is acceptable for the isolated prototype.** It is a coarse static secret and is not the preferred production route; move to Entra ID.
7. **The model deployment name.** The deployment points to the chosen model/version/configuration.
8. **A Foundry project endpoint.** The path identifies a project inside the resource.
9. **Portal generation, SDK major version, imports, constructor, and endpoint shape.** New and classic samples must not be mixed.
10. **`403 Forbidden`.** Authentication can succeed while authorization fails.
11. **`AIProjectClient`**
12. **`DefaultAzureCredential`**
13. **`api/projects/<project-name>`**
14. **The deployment name**, for example `claims-assistant`
15. **An OpenAI-compatible client associated with the project.**

</details>

## Prepared exit check

Use this only during a future study or review session. Without notes, explain all eight statements:

1. A subscription, resource group, Foundry resource, project, and deployment are different scopes or components.
2. A Foundry resource is the shared governance and platform boundary; a project is an application workspace.
3. A deployment makes a configured model callable, and requests commonly use the deployment name.
4. Control-plane operations manage the platform; data-plane operations invoke runtime capabilities.
5. Project, OpenAI-compatible, deployment-scoped, and service-specific endpoints serve different operations.
6. Authentication proves identity; RBAC authorization grants permissions at a scope.
7. API keys suit quick prototypes, while Entra ID and managed identity are preferred for production.
8. New and classic Foundry SDK samples must not be mixed without checking versions and endpoint shapes.

**Suggested future study-session heuristic:** 8/8 correct explanations plus at least 12/15 practice answers. This is not an official Microsoft passing threshold.

## Exam-readiness gaps

Matching research: [`../../research/gaps/foundry-resources-projects-endpoints-authentication.md`](../../research/gaps/foundry-resources-projects-endpoints-authentication.md)

Accepted gaps addressed by this topic file:

- **Azure and Foundry hierarchy gap — High confidence:** the full scope chain is consolidated.
- **Resource/project/deployment gap — High confidence:** responsibilities are separated.
- **Deployment-name gap — High confidence:** request semantics are explicit.
- **Endpoint-selection gap — High confidence:** endpoint types have a decision rule.
- **Authentication/RBAC gap — High confidence:** identity, permission, `401`, and `403` are separated.
- **Control/data-plane gap — High confidence:** setup and runtime operations are compared.
- **Current/classic freshness gap — High confidence:** package generations and retired guidance are flagged.
- **Scenario/code practice gap — Medium confidence:** original practice and answer keys are stored.

The research-session deliverables are complete. These exercises are reserved for a future study session; no learner status is inferred from their creation.

## Official sources

- AI-901 study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901
- Get started with AI in Azure: https://learn.microsoft.com/en-us/training/modules/get-started-with-ai-in-azure/
- Understand Azure: https://learn.microsoft.com/en-us/training/modules/get-started-with-ai-in-azure/2-what-is-azure
- Developing AI apps on Azure: https://learn.microsoft.com/en-us/training/modules/get-started-with-ai-in-azure/3-develop-ai-apps
- Microsoft Foundry for AI: https://learn.microsoft.com/en-us/training/modules/get-started-with-ai-in-azure/4-microsoft-foundry
- Using Microsoft Foundry endpoints: https://learn.microsoft.com/en-us/training/modules/get-started-with-ai-in-azure/5-endpoints
- Authentication and authorization: https://learn.microsoft.com/en-us/azure/foundry/concepts/authentication-authorization-foundry
- Foundry RBAC: https://learn.microsoft.com/en-us/azure/foundry/concepts/rbac-foundry
- Foundry SDKs and endpoints: https://learn.microsoft.com/en-us/azure/foundry/how-to/develop/sdk-overview
- Foundry model endpoints: https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/endpoints
- Managed compute overview: https://learn.microsoft.com/en-us/azure/foundry/concepts/managed-compute-overview

## External gap-research sources

See the matching gap-research file. External candidate evidence is not used as official teaching content here.

## Metadata

- Verified on: July 29, 2026
- Official blueprint checked on: July 29, 2026
- Research material status: Complete
- Study-session status: Not started
