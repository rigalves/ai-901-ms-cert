# Foundry resources, projects, deployments, endpoints, and authentication — Exam-readiness gaps

Research date: **July 29, 2026**

## Official scope

This is a cross-cutting foundation for the AI-901 implementation domain. It directly supports the official objectives that require a learner to:

- Select model deployment options and configuration parameters.
- Deploy a model and interact with it in the Microsoft Foundry portal.
- Build lightweight applications for generative AI, agents, text analysis, speech, computer vision, and information extraction.

The current study guide also expects familiarity with Azure resources, REST APIs, SDKs, and command-line tools. This file does not add a separate exam domain.

## Official material reviewed

- AI-901 study guide: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-901
- Get started with AI in Azure: https://learn.microsoft.com/en-us/training/modules/get-started-with-ai-in-azure/
- Understand Azure: https://learn.microsoft.com/en-us/training/modules/get-started-with-ai-in-azure/2-what-is-azure
- Developing AI apps on Azure: https://learn.microsoft.com/en-us/training/modules/get-started-with-ai-in-azure/3-develop-ai-apps
- Microsoft Foundry for AI: https://learn.microsoft.com/en-us/training/modules/get-started-with-ai-in-azure/4-microsoft-foundry
- Using Microsoft Foundry endpoints: https://learn.microsoft.com/en-us/training/modules/get-started-with-ai-in-azure/5-endpoints
- Module exercise: https://learn.microsoft.com/en-us/training/modules/get-started-with-ai-in-azure/6-exercise
- Authentication and authorization: https://learn.microsoft.com/en-us/azure/foundry/concepts/authentication-authorization-foundry
- Foundry RBAC: https://learn.microsoft.com/en-us/azure/foundry/concepts/rbac-foundry
- Foundry SDKs and endpoints: https://learn.microsoft.com/en-us/azure/foundry/how-to/develop/sdk-overview
- Foundry model endpoints: https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/endpoints
- Managed compute overview: https://learn.microsoft.com/en-us/azure/foundry/concepts/managed-compute-overview

## Coverage assessment

The Learn module establishes the Azure hierarchy and introduces Foundry resources, projects, endpoints, keys, Microsoft Entra ID, and client applications. Its end-to-end sequence is appropriate for a beginner:

`tenant → subscription → resource group → Foundry resource → project → model or tool → endpoint + credential → client application`

The material is intentionally compact. It places several endpoint styles and authentication choices close together without a reusable decision rule. Product documentation provides the missing distinctions: resource versus project scope, control plane versus data plane, deployment name versus model name, project endpoints versus OpenAI-compatible endpoints, and authentication versus authorization.

## Gaps

### The Azure and Foundry hierarchy is easy to flatten into one object

- **Type:** Underexplained and terminology gap
- **Evidence:** The Learn module introduces tenants, subscriptions, resource groups, resources, and projects in separate units, but does not consolidate them into one exam-ready hierarchy.
- **Confidence:** High
- **Why it matters:** Billing, quota, governance, lifecycle, assets, and application access belong at different scopes. A project is not another subscription or a separate Foundry resource.
- **Study material needed:** One hierarchy diagram and a component responsibility table.

### Foundry resource, project, and model deployment have different responsibilities

- **Type:** Terminology gap and practice gap
- **Evidence:** Current product documentation defines the resource as the administrative, security, quota, monitoring, and model-hosting boundary; a project as a workspace within that resource; and a deployment as a callable configured model.
- **Confidence:** High
- **Why it matters:** A learner may incorrectly place shared security or quota at project scope, or confuse a deployed model with the project that consumes it.
- **Study material needed:** A three-level comparison and scenario-selection questions.

### Deployment name and model identity are not interchangeable

- **Type:** Implementation gap and practice gap
- **Evidence:** A deployment contains a model name, model version, capacity or provisioning choice, content-filter configuration, and a deployment name. Requests address the deployment name.
- **Confidence:** High
- **Why it matters:** SDK parameters named `model` commonly expect the deployment name in Azure, not merely a catalog model identifier.
- **Study material needed:** Explicit request-flow language and one code-recognition question.

### Endpoint examples need a selection rule

- **Type:** Underexplained, implementation gap, and terminology gap
- **Evidence:** The Learn endpoint unit shows project-level and deployment-scoped requests in one short lesson. Current SDK documentation also distinguishes the project endpoint from the OpenAI-compatible resource endpoint and service-specific tool endpoints.
- **Confidence:** High
- **Why it matters:** The URL is selected by the operation and client, not by a rule that every Foundry operation uses one universal endpoint.
- **Study material needed:** A compact endpoint decision table that stays at recognition level.

### Authentication and authorization are taught together but fail differently

- **Type:** Underexplained and practice gap
- **Evidence:** Microsoft documentation distinguishes proving identity from receiving permission through Azure RBAC. It also distinguishes invalid credentials or tokens from missing role assignments.
- **Confidence:** High
- **Why it matters:** A valid identity can still receive `403 Forbidden`; an invalid or missing credential commonly produces `401 Unauthorized`.
- **Study material needed:** A credential-versus-permission mental model and troubleshooting scenarios.

### API keys and Microsoft Entra ID are not equivalent security choices

- **Type:** Security and implementation gap
- **Evidence:** API keys are resource-scoped static secrets with coarse access. Microsoft recommends Entra ID for production because it supports managed identities, conditional access, auditing, and granular RBAC.
- **Confidence:** High
- **Why it matters:** “Simpler” does not mean “recommended for production,” and key-based access does not express a per-user role.
- **Study material needed:** A concise selection table: key for quick isolated prototypes; Entra ID and managed identity for production.

### Control-plane and data-plane operations need explicit recognition

- **Type:** Missing consolidation and terminology gap
- **Evidence:** Current product documentation formally separates management operations from runtime operations. The introductory Learn module discusses both but does not name and compare the planes.
- **Confidence:** High
- **Why it matters:** Creating a resource, project, role assignment, connection, or deployment is different from invoking a model, agent, evaluation, or analyzer.
- **Study material needed:** A two-column comparison with original scenarios.

### Current and classic Foundry samples can be mixed accidentally

- **Type:** Freshness and terminology gap
- **Evidence:** The new Foundry portal uses `azure-ai-projects` 2.x and the current project-endpoint shape. Foundry classic samples can use 1.x and older object models. Microsoft also continues a gradual rename from Azure AI roles to Foundry roles.
- **Confidence:** High
- **Why it matters:** Combining new and classic package versions, endpoint formats, or role names can produce code and terminology that look plausible but do not fit together.
- **Study material needed:** A freshness warning, not a migration lab.

### Deprecated Azure AI Inference SDK examples remain a freshness risk

- **Type:** Freshness gap
- **Evidence:** Microsoft states that the Azure AI Inference beta SDK is deprecated and retires on August 26, 2026, with migration to the GA OpenAI/v1 API.
- **Confidence:** High
- **Why it matters:** Older examples may still appear in search results and study notes shortly before the learner's target exam date.
- **Study material needed:** Recognition-level warning to prefer current Foundry SDK and OpenAI/v1 guidance.

### The official module needs more scenario and code-recognition practice

- **Type:** Practice gap and reported exam emphasis
- **Evidence:** The module supplies an exercise, but little stored practice for choosing scope, endpoint, credential, role, or deployment identifiers. Several 2026 candidate accounts emphasize Foundry and SDK familiarity, while Microsoft Q&A explains that coding items can ask candidates to identify a missing method rather than complete a lab.
- **Confidence:** Medium for SDK and workflow preparation; Low for question frequency
- **Why it matters:** The official objectives already justify recognition practice. Candidate reports do not justify invented weighting.
- **Study material needed:** Original scenarios and short Python recognition items with an answer key.

## Study-session assets created

Created **July 29, 2026**:

- [`../../docs/topics/foundry-foundation.md`](../../docs/topics/foundry-foundation.md)
  - Azure-to-Foundry hierarchy and scope table
  - resource, project, deployment, endpoint, and credential distinctions
  - control-plane versus data-plane comparison
  - endpoint and authentication decision tables
  - current role and terminology guidance
  - minimal project-client recognition example with C# mental mappings
  - original scenarios and code-recognition questions
  - answer key and future study-session exit check

**Research status:** Complete. The identified material exists and is linked. Learner understanding and Microsoft Learn progress are intentionally unchanged.

## Claims not accepted

- A Foundry resource and a Foundry project are interchangeable.
- Each project has its own Azure subscription, billing boundary, or independent resource quota.
- The model catalog name always belongs in an SDK's `model` argument; Azure requests often require the deployment name.
- Every Foundry API uses one universal endpoint.
- API-key authentication provides granular Azure RBAC or per-user auditing.
- Successfully authenticating automatically grants permission to perform the requested operation.
- `401 Unauthorized` and `403 Forbidden` represent the same failure.
- New Foundry and Foundry classic SDK samples can be combined safely without checking package versions and endpoint shapes.
- The deprecated Azure AI Inference beta SDK is the preferred route for new preparation.
- Candidate anecdotes prove the number or distribution of Foundry questions.
- Any dump-derived claim about real exam questions.

## Suggested future study checkpoint

Use the prepared checkpoint in `docs/topics/foundry-foundation.md` during a later study or review session. That session may evaluate whether the learner can:

1. Trace the hierarchy from tenant to client request.
2. Separate resource, project, and deployment responsibilities.
3. Select a project, OpenAI-compatible model, or service-specific endpoint for a scenario.
4. Explain why a request uses the deployment name.
5. Distinguish authentication from RBAC authorization and `401` from `403`.
6. Choose API key, user identity, service principal, or managed identity appropriately.
7. Separate control-plane setup from data-plane runtime operations.
8. Avoid mixing current and classic SDK guidance.

This checkpoint is stored as a handoff. It is not administered during gap research.

## External evidence reviewed

- First-hand AI-901 beta account emphasizing Python, SDK, and Foundry familiarity, April 29, 2026: https://www.linkedin.com/pulse/career-trajectory-issue-165-free-certified-program-20-carla-qb2zc
- First-hand beta account naming Microsoft Foundry and Python SDKs among preparation areas, June 2026: https://www.linkedin.com/in/robert-green10
- First-hand beta account emphasizing lightweight application objectives, May 2026: https://www.linkedin.com/posts/alanro_ai901-ai103-ai900-activity-7452955327089672192-JoGp
- Microsoft Q&A explanation that fundamentals exams do not use labs and coding items may ask for a missing method, June 19, 2026: https://learn.microsoft.com/en-us/answers/questions/5924630/does-the-ai-901-exam-include-a-lab-performance-bas
- Official exam page, study guide, Learn modules, and product documentation remain the authority for scope and technical guidance.
