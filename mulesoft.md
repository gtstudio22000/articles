# Mulesoft 

MuleSoft is a company known for its Anypoint Platform, which helps organizations integrate various systems, services, and applications. The platform is used to design, build, and manage APIs and integrations.

The Anypoint Platform designs, builds, and manages APIs with MuleSoft's API Manager. API Manager lets you set and manage API policies, regulate access, collect analytics, and ensure APIs are utilised properly.

In MuleSoft's API Manager, a "instance" is a deployed or operating API you've defined. Each instance may represent a particular API version or environment (development, testing, production).

The API Manager lets you configure Anypoint Platform-deployed APIs. This allows:

- Control Traffic—Throttling, rate limiting, and other policies can limit API calls and who can use them.
- OAuth 2.0 policies can restrict API access to authorised applications and users.
- Monitor and analyse - Anypoint Analytics lets you see how your API is utilised, check for issues, and improve performance.
- Apply Policies—You can apply pre-built API policies to conduct typical API management tasks. You can also customise policies.
- Versions and SLAs - Manage API versions and SLAs for consumer applications.
In essence, MuleSoft's API Manager represents a deployment of an API and provides tools to ensure its secure, efficient, and organizationally aligned use.

# API Manager Instance

The term "instance" in MuleSoft's API Manager refers to a specific deployment or operational version of an API.
API Manager lets you manage System, Process, and Experience API settings and configurations. API Manager considers each API deployment a "instance".

A System API deployed on the Anypoint Platform counts as one API Manager instance. Multiple versions or environments (e.g., dev, test, prod) of the same System API are usually considered separate instances in API Manager.

When deployed, a System API with numerous endpoints is treated as a single API Manager instance. Each API endpoint is not an instance.

Consider a System API that interacts with a user database. The API may have endpoints like:

```

GET /users: Get users.
POST /users: Create a user. 
GET /users/{id}: Retrieve particular user details.
PUT /users/{id}: Update a specific user.
DELETE /users/{id}: Remove a specific user.

```

Multiple endpoints handle distinct actions, yet they're all part of the same API. This API is deployed as a single instance in the API Manager, regardless of endpoint count.

## vCores

A vCore (virtual core) is a computational capacity unit used to measure and distribute resources to CloudHub apps and APIs in MuleSoft's Anypoint Platform.

Your Mule application or API deployment to CloudHub specifies the number of vCores. The number of vCores you assign determines your application's computing power and memory. This lets you scale your app as needed.

Some MuleSoft vCores facts:

Resource Allocation: Each vCore allocates CPU and memory. For instance, 0.1 vCore has 500 MB of RAM, 1 has 1.5 GB, and 2 has 3.5 GB. These values are indicative; allocations may vary.
Scalability: Add vCores to accommodate heavy traffic or resource needs in your application or API. Less vCores might be employed for lighter workloads or non-production situations.
Cost: Number of vCores per application affects cost. Higher computational capacity and prices come with more vCores.
Workers: CloudHub lets you choose how many workers (application instances) to operate. Workers receive CPU and memory according to the vCore configuration. Running several workers increases vCore utilisation. Two workers running an application that uses 1 vCore would use 2 vCores.
Runtime Manager: Anypoint Platform's Runtime Manager lets you track programme consumption and performance, including vCores.
MuleSoft offers dedicated vCores for applications that need isolation from multitenant CloudHub. Security and compliance often need this.
Understanding vCores and assigning them according to your application's demands is crucial for Anypoint Platform performance and cost-efficiency.


## The Runtime Manager

The Runtime Manager manages the deployment lifecycle of Mule apps, enabling users to deploy, start, stop, and restart them.
Monitoring and Alerts: Shows Mule application health, performance, and metrics. It lets users configure warnings for certain scenarios, such as an application's error rate exceeding a threshold.
Logging: Views and searches Mule application logs.
Schedules Mule applications with time-based flows.
Scaling: Runtime Manager autoscales CloudHub apps.
Infrastructure Management: Configures and manages CloudHub infrastructure like VPCs and VPNs.


## The API Manager 

The API Manager manages the entire lifecycle of APIs, from design to retirement. This includes API version management.
API Policy Application: Allows users to apply default or custom API policies. OAuth authentication, CORS, rate limitation, etc.
API Analytics: Shows API usage, including requests, response times, and errors.
Tiers and Application Access: Manages SLA and application access levels. For instance, you can limit users to 1,000 or 10,000 API calls per hour.
Contract management: API consumer-provider contracts. Tracking which apps consume which APIs is required.
API Proxy: Creates a proxy application between API consumers and backend implementations. The proxy enforces policies and collects analytics.

## MuleSoft's pricing model

For its Anypoint Platform, which includes the API Manager, MuleSoft charges per instance, although its pricing approach is more complicated. Some items to note:

When installing apps on CloudHub, MuleSoft's price is heavily based on vCores. Mule application vCores are a cost issue if your API contains a backend Mule application, as it commonly does.
API Gateway Runtime: If you're using the on-premises Mule runtime with API Gateway features, pricing may be based on runtime cores or cluster nodes rather than API Manager instances.
API Calls: The API Manager may not have a per-instance fee, but API calls and data processing may. Using Anypoint Platform's analytics and monitoring features makes this important.
MuleSoft has subscription-based licencing models. The details of a subscription differ. Some licencing models limit API requests or other metrics.
VPC, VPN, Anypoint Monitoring, Anypoint Visualizer, and other Anypoint Platform capabilities might also affect costs.
The entire cost can also depend on support levels (Gold, Platinum) and Service Level Agreements (SLAs).

