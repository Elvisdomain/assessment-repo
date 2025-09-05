source-code-repo/
│── .github/
│    └── workflows/
│         └── ci-cd.yml
│
│── services/
│    ├── account-service/
│    │    ├── src/
│    │    ├── pom.xml
│    │    ├── Dockerfile
│    │    └── sonar-project.properties
│    │
│    ├── audit-service/
│    │    ├── src/
│    │    ├── pom.xml
│    │    ├── Dockerfile
│    │    └── sonar-project.properties
│    │
│    ├── payment-service/
│         ├── src/
│         ├── pom.xml
│         ├── Dockerfile
│         └── sonar-project.properties
│
└── pom.xml   




⚙️ CI/CD Workflow

The pipeline is designed with three primary stages:

1. SonarCloud Analysis (Code Quality & Security)
Runs on push and pull_request events to the main branch.

Steps:
•	Checkout source code

•	Setup JDK 17 and Maven build

•	Run SonarCloud static analysis

•	Verify Quality Gate status (build fails if it doesn’t pass).


2.  Synk Code Vulnerability Scan
•	Ensures deployment only happens if Snyk passes.
•	Used snyk/actions/maven (best for Maven-based projects).
•	Enforced high severity threshold (--severity-threshold=high) → pipeline fails on high/critical       vulnerabilities.


3. ECR Deployment (Container Build & Push)
Triggered only on push events to develop.
Steps:
•	Build .jar files for each microservice (account-service, payment-service, audit-service)
•	Create Docker images with Git commit hash as tag
•	Authenticate with Amazon ECR
•	Push images to their respective repositories


🔑 Key Features
•	Matrix builds → Each service (account-service, audit-service, payment-service) builds/tested independently.

•	Each service has its own Dockerfile under services/<service>/Dockerfile.

•	SonarCloud → Independent project keys for each service.

•	Dependency awareness →

•	payment-service build runs only after account-service and audit-service are green ✅ 



✅ Dependency Management
•	Account and audit must finish before payment.

•	Payment-depends waits on both. This way, payment-service builds only after account-service and audit-service artifacts are built.


👉 This setup ensures:
•	Independent builds of each service.

•	SonarCloud scanning per service.

•	Docker images pushed per service with proper tagging





