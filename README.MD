# MCP Server CI/CD Project – Overview and Key Checkpoints

## Objective

Establish a **secure, automated CI/CD pipeline** for MCP (Model Context Protocol) server development, enabling consistent testing, vulnerability scanning, image signing, and deployment. The pipeline should support both **Python** and **Node-based** MCP servers and integrate **DevSecOps** best practices.

---

## Project Overview

The project aims to design and deploy a **reusable CI/CD framework** that validates MCP server functionality, protocol compliance, and security posture before release. It must automate **build, test, scan, and deploy** steps, supporting multiple transports (HTTP/SSE or stdio).

The pipeline will focus on:

* **Code quality and static analysis**
* **MCP protocol and interoperability testing**
* **Dependency and container security**
* **Continuous delivery of signed, compliant builds**

---

## Main Checkpoints / Deliverables

### 1. Environment Setup

* Define supported environments: **Python 3.11+**, **Node 20+**.
* Configure **GitHub Actions runners** or equivalent CI agents.
* Establish **artifact storage** (SBOMs, signed images).

### 2. Pipeline Architecture

* Define stages: **build → test → scan → sign → deploy**.
* Create job templates for **Python** and **Node** MCP servers.

### 3. Security & Compliance Requirements (DevSecOps)
#### Phase 1 Pipeline
| Area | Tooling / Requirement | Enforcement |
| :--- | :--- | :--- |
| **Build & test** | **pytest**, **coverage.py**, **flake8**, **mypy** | Enforec |
| **SAST** (Static Analysis) | **Bandit** (Python), **Semgrep** (Maybe)(General) | Enforce failure on high/critical vulnerabilities |
|  **DAST** | **OWASP ZAP**, **Nikto** (basic web server scan) | Enforce |
| **SCA (Dependency Audit)** | **pip-audit** (Python), **OWASP Dependecy Check** (Maybe) | Enforce failure on high/critical vulnerabilities |
| **Secret Scanning** | **Gitleaks**, **TruffleHog** | Enforce |


### Phase 2 Pipeline
| Area | Tooling / Requirement | Enforcement |
| :--- | :--- | :--- |
| **SBOM Generation** | **Syft** | Enforce |
| **Container Scans** | **Trivy** (image, filesystem, misconfig, secrets), **Grype** (Image/filesystem CVEs), **Dockle** (Dockerfile lint)| Enforce failure on high/critical vulnerabilities |
| **Artifact Security** | **SBOM** generation (**Syft**) and **signing** (**Cosign**) | Signed and verifiable images for every release |

### Phase 3 Pipeline
| Area | Tooling / Requirement | Enforcement |
| :--- | :--- | :--- |
| **Signing & Provenance** | **Sigstore Cosign** (container + image signing), Provenance Attestation tool | Enforce |

### 4. MCP Protocol Validation

* Integrate **SDK-based smoke tests** (list tools/resources, call tool).
* Verify compliance with **Model Context Protocol specifications**.

### 5. Quality Assurance

* Include **linting, type checks**, and **unit/integration tests**.
* Record **coverage metrics** and publish **test results**.

### 6. Continuous Deployment

* Automate image push to **GHCR** (GitHub Container Registry) or internal registry after passing all gates.
* *Optional:* Deploy to a **Kubernetes test cluster** for live validation.

### 7. Governance

* Establish **approval gates** before production deployment.
* Implement **artifact retention** and an **audit trail** for signed builds.
* Create **security policy documentation** for pipeline use.

---

## Success Metrics

* **100% automated** build, test, and scan coverage.
* **Zero high/critical vulnerabilities** at release.
* **Successful MCP compliance checks** across environments.
* **Signed and verifiable images** for every release.




# Work Zone
## 1. Build and Test
The purpose of this step is to build code by seting up dependencies, compiling packages, running automated tests and performing quality checks in an effort to catch bugs early, maintain quality and ensure app builds and runs before deployment.
### Tools
1. Pytest
* Runs automated test located in test files and reports pass/fail to ensure all code features run correctly after a change.
    * Measure success using pytest-cov plugin
2. coverage.py
* Measures how much your code is actually tested during a test and returns a report to help show completeness of test.
3. Flake8 
* Checks code style, syntax and potential errors (indentation and spacing, unused variables or imports)
* Keep dependencies up to data in requirements.txt (pip install -r requirements.txt during build)
* ex: 
    
    1. Install dependencies
    pip install -r requirements.txt

    2. Run static code checks
    flake8 .

    3. Run tests with coverage
    pytest --cov=.

    4. Upload or view test reports
    coverage report -m
    