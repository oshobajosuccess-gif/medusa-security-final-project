\# Security Assessment of a Local Medusa Commerce API



\## A Technical Cybersecurity Assessment of Authentication Controls and API Security



\*\*Student:\*\* Oshobajo Success

\*\*Project:\*\* Final Individual Cybersecurity Research/Technical Report

\*\*Technology:\*\* Medusa Commerce Platform 2.19.0

\*\*Environment:\*\* Local Windows development environment

\*\*Backend:\*\* Node.js / Express

\*\*Port:\*\* 9000

\*\*Repository:\*\* https://github.com/oshobajosuccess-gif/medusa-security-final-project.git

\*\*Date:\*\* September 2026



\---



\# Abstract



This project presents a practical cybersecurity assessment of a locally deployed Medusa commerce application. The assessment focused primarily on API authentication, authorization controls, and basic HTTP security hardening.



The application was deployed in a controlled local development environment using Medusa version 2.19.0. Security tests were performed against selected application endpoints to determine how the API responds to unauthenticated requests, valid publishable API credentials, and invalid credentials.



The assessment demonstrated that the `/store/products` endpoint rejects requests without a publishable API key and also rejects invalid publishable keys. A request containing a valid publishable API key successfully returned a `200 OK` response. The `/admin/products` endpoint returned `401 Unauthorized` when accessed without authentication, demonstrating enforcement of administrative access controls.



A security-header review also identified hardening opportunities, including exposure of the Express framework through the `X-Powered-By` response header and the absence of several commonly recommended security headers in the tested response.



The results demonstrate that authentication and authorization controls are functioning on the tested endpoints, while additional security hardening could improve the application's defensive posture.



\---



\# 1. Introduction



Modern e-commerce applications expose APIs that handle sensitive operations such as product management, customer information, orders, authentication, and administrative functions. Weak authentication or authorization controls can allow unauthorized users to access protected functionality.



This project investigates the security of a locally deployed Medusa commerce application. The assessment was performed in a controlled environment owned and operated for educational purposes.



The primary focus was to verify whether selected API endpoints properly distinguish between public, authenticated, and administrative requests.



The assessment also examined HTTP response headers to identify basic security-hardening opportunities.



\---



\# 2. Project Objectives



The objectives of the assessment were:



1\. Deploy and operate the Medusa commerce application locally.

2\. Verify that the backend API is reachable and functioning.

3\. Test API access without the required publishable API key.

4\. Test API access using a valid publishable API key.

5\. Test API behavior when an invalid publishable API key is supplied.

6\. Test administrative API access without authentication.

7\. Review HTTP response headers for basic security weaknesses.

8\. Preserve test results as evidence.

9\. Document findings and security recommendations.

10\. Publish the project evidence through a public GitHub repository.



\---



\# 3. System Environment



The assessment was conducted in a local Windows development environment.



\## 3.1 Application



The application used for the assessment was a Medusa commerce application running version 2.19.0.



\## 3.2 Backend



The backend was a Node.js application using Express.



The application was configured to listen on:



`http://localhost:9000`



\## 3.3 Testing Tools



The following tools were used during the assessment:



\* Windows PowerShell

\* cURL

\* Git

\* GitHub

\* Medusa CLI

\* Node.js

\* pnpm



The tests were performed against the local application rather than an external production system.



\---



\# 4. Security Assessment Methodology



The assessment followed a controlled black-box API testing approach.



The methodology consisted of the following stages:



\### Stage 1 — Availability Verification



The `/health` endpoint was queried to confirm that the backend was running and responding to HTTP requests.



\### Stage 2 — Authentication Testing



The `/store/products` endpoint was tested without an API key.



The same endpoint was then tested with a valid publishable API key.



\### Stage 3 — Invalid Credential Testing



The `/store/products` endpoint was tested with an intentionally invalid test key.



This was used to determine whether the application accepted arbitrary credentials.



\### Stage 4 — Administrative Authorization Testing



The `/admin/products` endpoint was requested without authentication.



The objective was to determine whether administrative functionality was accessible to an unauthenticated client.



\### Stage 5 — Security Header Review



HTTP response headers from the `/health` endpoint were examined for commonly recommended security controls.



\---



\# 5. Test Results



\## 5.1 Health Endpoint



\### Request



`GET /health`



\### Result



`HTTP/1.1 200 OK`



The server returned a successful response containing:



`OK`



\### Assessment



This confirmed that the backend application was running and accepting HTTP requests.



\### Evidence



Evidence was collected using cURL and retained as part of the project testing process.



\---



\# 6. Publishable API Key Authentication



\## 6.1 Request Without API Key



The `/store/products` endpoint was first requested without providing the required publishable API key.



\### Result



`HTTP/1.1 400 Bad Request`



The application returned a message indicating that a publishable API key was required.



\### Security Interpretation



This demonstrates that the endpoint does not accept an unauthenticated request when the required API credential is missing.



\### Evidence File



`apps/backend/evidence/store-products-no-key.txt`



\---



\## 6.2 Request With Valid API Key



The same endpoint was then tested using a valid publishable API key configured for the local Medusa application.



\### Result



`HTTP/1.1 200 OK`



The server successfully processed the request and returned the products response.



\### Security Interpretation



This demonstrates that the API correctly recognizes a valid publishable credential and allows the request to proceed.



The actual API key was not included in the report or repository.



\---



\# 7. Invalid API Key Testing



The `/store/products` endpoint was tested with a deliberately invalid test credential.



\### Request



`GET /store/products`



Header:



`x-publishable-api-key: invalid-test-key`



\### Result



`HTTP/1.1 400 Bad Request`



The server returned:



`A valid publishable key is required to proceed with the request`



\### Security Interpretation



The application did not simply check whether a credential existed. It also validated the supplied publishable key.



This is an important authentication-control result because an attacker cannot gain access merely by supplying an arbitrary value in the authentication header.



\### Evidence File



`apps/backend/evidence/store-products-invalid-key.txt`



\---



\# 8. Administrative Authorization Testing



The administrative products endpoint was tested without authentication.



\### Request



`GET /admin/products`



\### Result



`HTTP/1.1 401 Unauthorized`



The server returned:



`{"message":"Unauthorized"}`



\### Security Interpretation



The response demonstrates that the administrative endpoint rejects unauthenticated requests.



This is particularly important because administrative endpoints can expose privileged functionality and should not be accessible to anonymous users.



\### Evidence File



`apps/backend/evidence/admin-products-no-auth.txt`



\---



\# 9. HTTP Security Header Assessment



The `/health` endpoint was inspected using an HTTP HEAD request.



\### Result



The response included:



\* `X-Powered-By: Express`

\* `Content-Type`

\* `Content-Length`

\* `ETag`

\* `Date`

\* `Connection`

\* `Keep-Alive`



The tested response did not include several commonly recommended security headers, including:



\* `Content-Security-Policy`

\* `X-Content-Type-Options`

\* `X-Frame-Options`

\* `Strict-Transport-Security`



\### Finding



The `X-Powered-By: Express` header reveals the underlying web framework.



Additionally, the absence of several security headers represents a security-hardening opportunity.



\### Risk



The exposure of the framework is generally considered a low-severity information disclosure issue. It does not by itself provide unauthorized access to the application.



Missing security headers can reduce browser-side defensive protections, particularly when the application is deployed beyond a controlled local development environment.



\### Evidence File



`apps/backend/evidence/security-headers-health.txt`



\---



\# 10. Security Findings



\## Finding 1 — Publishable API Key Enforcement



\*\*Severity:\*\* Informational / Positive Control



The `/store/products` endpoint rejected requests without a publishable API key.



This demonstrates that the endpoint has an authentication requirement.



\---



\## Finding 2 — Invalid Publishable Key Rejected



\*\*Severity:\*\* Informational / Positive Control



The endpoint rejected an intentionally invalid publishable key.



This demonstrates that the application validates the supplied credential rather than accepting any arbitrary key value.



\---



\## Finding 3 — Administrative Endpoint Protected



\*\*Severity:\*\* Informational / Positive Control



The `/admin/products` endpoint returned `401 Unauthorized` when requested without authentication.



This demonstrates that administrative functionality is protected against unauthenticated access.



\---



\## Finding 4 — Security Header Hardening Opportunity



\*\*Severity:\*\* Low



The tested response exposed the Express framework through `X-Powered-By` and did not include several commonly recommended security headers.



This does not demonstrate a direct compromise, but it represents an area where the application's defensive configuration can be improved.



\---



\# 11. Recommendations



\## 11.1 Maintain API Authentication



Publishable API key enforcement should remain enabled for endpoints that require it.



API credentials should never be hard-coded into source code or committed to public repositories.



\---



\## 11.2 Protect Administrative Endpoints



Administrative routes should continue to require authenticated and appropriately authorized users.



Administrative privileges should follow the principle of least privilege.



\---



\## 11.3 Improve Security Headers



For deployment beyond local development, security headers should be reviewed and configured appropriately.



Possible controls include:



\* `Content-Security-Policy`

\* `X-Content-Type-Options`

\* `X-Frame-Options`

\* `Strict-Transport-Security`



The exact configuration should be tested against the application's functionality before production deployment.



\---



\## 11.4 Reduce Framework Disclosure



The `X-Powered-By` header can be disabled in an Express application to reduce unnecessary technology disclosure.



This is a hardening measure rather than a standalone solution to application security.



\---



\## 11.5 Protect Secrets



Environment variables and credentials should remain outside source control.



The project `.gitignore` configuration excludes local environment files, helping prevent accidental publication of secrets.



\---



\## 11.6 Continue Automated Security Testing



The authentication tests performed in this project should be incorporated into a repeatable security-testing process.



Automated tests can help detect regressions when application configuration or API behavior changes.



\---



\# 12. Limitations



This assessment was performed against a local development deployment.



The testing therefore does not represent a complete production penetration test.



The assessment focused on selected API authentication, authorization, and HTTP-header behaviors. It did not attempt to test every endpoint, business-logic vulnerability, dependency vulnerability, database security issue, or infrastructure configuration.



No destructive testing was performed.



The results should therefore be interpreted as evidence from the tested endpoints rather than proof that the entire application is free from vulnerabilities.



\---



\# 13. Evidence Management



Testing evidence was stored within the project repository.



Relevant evidence files include:



\* `apps/backend/evidence/store-products-no-key.txt`

\* `apps/backend/evidence/store-products-success.txt`

\* `apps/backend/evidence/store-products-invalid-key.txt`

\* `apps/backend/evidence/admin-products-no-auth.txt`

\* `apps/backend/evidence/security-headers-health.txt`



The evidence demonstrates the actual HTTP responses observed during testing.



Sensitive credentials were intentionally excluded from the evidence and repository.



\---



\# 14. GitHub Repository



The complete project and supporting evidence are available in the public GitHub repository:



https://github.com/oshobajosuccess-gif/medusa-security-final-project



The repository contains the application, documentation, and security-testing evidence.



The repository is intended to remain accessible without requiring authentication so that it can be reviewed as part of the project submission.



\---



\# 15. Conclusion



This project demonstrated a practical security assessment of a locally deployed Medusa commerce API.



The testing confirmed several positive security controls. The products endpoint rejected requests without a required publishable API key, rejected invalid credentials, and accepted a valid credential. The administrative products endpoint also rejected unauthenticated access with a `401 Unauthorized` response.



The assessment additionally identified security-hardening opportunities in the HTTP response headers. In particular, the application exposed the Express framework through the `X-Powered-By` header, while several commonly recommended security headers were absent from the tested response.



Overall, the assessment demonstrates the importance of validating authentication and authorization controls at the API layer and combining functional testing with security-hardening reviews.



The project also demonstrates an evidence-driven cybersecurity workflow: establish the test environment, perform controlled tests, capture results, document findings, recommend mitigations, and preserve the evidence in a version-controlled repository.



\---



\# References



1\. Medusa Documentation — https://docs.medusajs.com/

2\. Express.js Documentation — https://expressjs.com/

3\. OWASP Application Security Verification Standard (ASVS) — https://owasp.org/www-project-application-security-verification-standard/

4\. OWASP API Security Top 10 — https://owasp.org/www-project-api-security/

5\. Git Documentation — https://git-scm.com/doc



\---



\# Appendix A — Evidence Summary



| Test                                     | Expected Security Behavior | Observed Result        | Status               |

| ---------------------------------------- | -------------------------- | ---------------------- | -------------------- |

| `/health`                                | Application responds       | 200 OK                 | Pass                 |

| `/store/products` without key            | Reject request             | 400 Bad Request        | Pass                 |

| `/store/products` with valid key         | Allow valid request        | 200 OK                 | Pass                 |

| `/store/products` with invalid key       | Reject invalid credential  | 400 Bad Request        | Pass                 |

| `/admin/products` without authentication | Reject anonymous access    | 401 Unauthorized       | Pass                 |

| Security headers                         | Harden HTTP responses      | Several headers absent | Improvement Required |



\# Appendix B — Repository Evidence



GitHub repository:



https://github.com/oshobajosuccess-gif/medusa-security-final-project



The repository contains the implementation and captured security-test evidence supporting the findings presented in this report.



