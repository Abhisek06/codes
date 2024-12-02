Here is an ADR in the requested format:

Problem Statement

The SolaP2ETL backend and Angular frontend need to be hosted in a suitable environment. The hosting solution must support Angular OIDC onboarding (Kerberos is unsupported) and ensure maintainability, scalability, and minimal configuration overhead.

Options

Option 1: Host Both SolaP2ETL and Angular on Webstax

Host Angular using an Apache server on Webstax by creating Docker images for both Angular and SolaP2ETL.

Pros

	•	Webstax supports containerized applications, making it compatible with Docker.
	•	Unified environment for backend and frontend simplifies deployment.
	•	Minimal CORS and HTTPS issues.

Cons

	•	Requires effort in creating and managing Docker images.
	•	Apache server configuration for Angular adds complexity.

Option 2: Host on Treadmill

Directly host on Treadmill, which requires additional configurations.

Approach 2.1: Host Angular on a separate server (e.g., Nginx or Python).

	•	Pros: Decouples frontend and backend, allowing independent scaling.
	•	Cons: High potential for CORS issues and uncertain HTTPS support.

Approach 2.2: Host both SolaP2ETL and Angular on the same server.

	•	Use a multi-module project where Angular build files are copied to SolaP2ETL/static/.
	•	Alternatively, create separate Docker images for Angular and reference them in SolaP2ETL’s Dockerfile.
	•	Pros: Simplifies deployment and avoids CORS issues.
	•	Cons: Increased build complexity and dependency management challenges.

Option 3: Deploy Angular on Webstax and SolaP2ETL on Treadmill Separately

Host Angular on Webstax and SolaP2ETL directly on Treadmill, each with its own load balancer.

Pros

	•	Separate environments optimize for frontend and backend needs.
	•	Independent scaling possible.

Cons

	•	Requires HTTPS configuration and handling of CORS issues.
	•	Increased network latency between the frontend and backend.

Option 4: Use Webfarm

Approach 4.1: Create a new Spring Boot app and host it on Webfarm with LegionUI integration.

	•	Pros: Leverages existing Webfarm infrastructure.
	•	Cons: Webfarm supports only Java 11 and has other limitations. Extra API calls increase latency.

Approach 4.2: Integrate LegionUI into SolaP2ETL and host on Webfarm.

	•	Pros: Centralized deployment.
	•	Cons: UI releases become less frequent due to tight coupling with the backend.

Decision

We choose Option 1: Host Both SolaP2ETL and Angular on Webstax.
	•	Using Docker images ensures compatibility with Webstax’s containerized environment.
	•	Unified hosting reduces CORS and HTTPS configuration overhead.
	•	Apache server integration provides a robust solution for Angular.

Rationale:
This option balances scalability, maintainability, and simplicity while addressing the project’s constraints and requirements.
