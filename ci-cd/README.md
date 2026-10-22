# CI/CD Fundamentals — DevOps Learning Journey

## Learning Status

- **Theory:** Essential CI/CD fundamentals covered
- **Deployment strategies:** Rolling, Blue-Green, and Canary covered
- **Practical implementation:** To be reinforced during hands-on exercises
- **Next learning stage:** Jenkins fundamentals and practical implementation

---

## 1. What Is CI/CD?

CI/CD stands for Continuous Integration and Continuous Delivery or Continuous Deployment.

It is a set of software development practices that helps teams automate the process of integrating code, building applications, testing changes, and delivering or deploying software.

### Why Do Companies Use CI/CD?

Without CI/CD, teams may have to perform builds, tests, and deployments manually. This can be slow, inconsistent, and error-prone.

CI/CD helps teams:

- Release software faster.
- Detect problems early.
- Reduce repetitive manual work.
- Improve consistency and reliability.
- Automate software testing and deployment.
- Make software releases more predictable.

**Real-world example:** A developer changes the code of an online shopping application and pushes it to GitHub. An automated pipeline builds the application, runs tests, and prepares it for deployment.

---

## 2. Continuous Integration (CI)

Continuous Integration is the practice of frequently integrating code changes into a shared repository and using automated builds and tests to detect problems early.

### How CI Works

1. A developer writes or modifies code.
2. The developer pushes the changes to a shared repository.
3. A CI system retrieves the code.
4. The system builds the application.
5. Automated tests run.
6. The results are reported to the team.

If the build or tests fail, the team investigates and fixes the problem.

**Example:** A developer introduces a coding error. The automated build or tests detect the problem before the change progresses further through the release process.

---

## 3. Continuous Delivery vs. Continuous Deployment

These practices extend automation beyond code integration.

### Continuous Delivery

Continuous Delivery automatically builds, tests, and prepares software so it is ready for release.

Production deployment may require a manual approval or release decision.

### Continuous Deployment

Continuous Deployment automatically releases every qualifying change to production after all required checks pass, without a manual production-release approval.

### Key Difference

| Continuous Delivery | Continuous Deployment |
|---|---|
| Software is kept ready for release. | Qualifying changes are automatically released to production. |
| Production release may require human approval. | No manual production-release approval is required. |
| Deployment to production can still be automated when authorized. | Production deployment is part of the automated flow. |

**Important:** Both practices use automation. The key distinction is whether production release requires a separate human decision.

---

## 4. What Is a CI/CD Pipeline?

A CI/CD pipeline is a sequence of stages that automates the process of building, testing, packaging, and delivering or deploying software.

A typical pipeline is:

**Code → Checkout → Build → Test → Package → Deploy**

The exact stages depend on the application and the team's requirements.

### Typical Pipeline Stages

#### 1. Code

Developers write application code and push changes to a source-code repository such as GitHub.

#### 2. Checkout

The CI/CD tool retrieves the required source code from the repository.

If checkout fails, the pipeline normally cannot proceed with stages that depend on that code.

#### 3. Build

The application is compiled or otherwise prepared for release.

For example, a Java application may be compiled and packaged.

#### 4. Test

Automated tests check whether the application behaves as expected.

#### 5. Package

The application is prepared as a deployable artifact, such as a JAR file or Docker image.

#### 6. Deploy

The application is released to an environment such as testing, staging, or production.

Not every pipeline has a separate stage for each activity. Some stages may be combined.

---

## 5. What Happens When a Pipeline Fails?

A pipeline can fail during checkout, build, testing, packaging, or deployment.

### Common Failures

| Stage | Example problem |
|---|---|
| Checkout | Repository unavailable or authentication failure |
| Build | Compilation error or missing dependency |
| Test | One or more automated tests fail |
| Package | Packaging error or invalid configuration |
| Deployment | Permission, networking, configuration, or infrastructure problem |

### What Happens After Failure?

1. The pipeline reports the failed stage.
2. The team examines the logs and error messages.
3. The responsible engineer investigates the cause.
4. The issue is corrected.
5. The pipeline is run again.

A failed stage normally blocks dependent stages, depending on the pipeline configuration.

**Important:** A pipeline failure does not always mean the application code is faulty. Failures can also result from credentials, unavailable services, network problems, configuration errors, or infrastructure issues.

---

## 6. What Is Jenkins, and How Does It Relate to CI/CD?

Jenkins is an automation server commonly used to implement CI/CD workflows.

These three terms mean different things:

- **CI/CD:** The software development and delivery practices.
- **Pipeline:** The workflow consisting of stages and steps.
- **Jenkins:** A tool that can execute and automate that workflow.

Jenkins is not CI/CD itself. Other tools can also implement CI/CD, including GitHub Actions.

Pipelines can run on local machines for learning and testing. In production, they commonly run on dedicated servers, virtual machines, containers, or cloud infrastructure.

We will study Jenkins and its practical implementation in the next learning stage.

---

## 7. CI/CD Triggers

A trigger determines when a pipeline starts.

### Manual Trigger

A user explicitly starts a pipeline.

**Example:** An engineer clicks the Build Now button in Jenkins.

### Webhook

A webhook allows a source-code platform to send an event notification to a CI/CD tool.

**Example:** A developer pushes code to GitHub. GitHub sends a notification to Jenkins, which can start the configured pipeline.

A webhook notification does not itself mean that someone approved the deployment. Approval requirements are separate pipeline controls.

### Poll SCM

Poll SCM means the CI tool periodically checks the source-code repository for changes.

When it detects qualifying changes, it can trigger a build.

**Example:** Jenkins checks GitHub at configured intervals and starts a build when it detects a new change.

### Scheduled Build / Build Periodically

A scheduled build runs according to a configured schedule, even when no source-code changes have occurred.

**Example:** A pipeline runs every night to perform a scheduled verification.

### Pull Request Trigger

A pipeline runs when a pull request is created or updated, depending on the configuration.

**Purpose:** Validate proposed code changes before they are merged into the main branch.

### Cron Syntax

Cron syntax is a format used by many tools to define schedules.

It is not a separate trigger type. It specifies when a scheduled job or polling operation should run.

### Trigger Comparison

| Trigger | When it runs |
|---|---|
| Manual | When a user starts it |
| Webhook | When a configured event notification is received |
| Poll SCM | When polling detects qualifying repository changes |
| Scheduled build | At configured times, whether or not code changed |
| Pull request trigger | When a configured pull request event occurs |

---

## 8. Source Code vs. Build Artifact

### Source Code

Source code consists of the original files written and maintained by developers.

Example: Java source files stored in a GitHub repository.

### Build Artifact

A build artifact is an output produced by the build or packaging process that can be stored or deployed.

Examples:

- Java application: JAR file
- Frontend application: Build output such as a `dist/` directory
- Containerized application: Docker image

### Where Are Artifacts Stored?

Depending on the project, artifacts can be stored in:

- The CI system or its build records.
- An artifact repository, such as Nexus or JFrog Artifactory.
- A container registry, such as Docker Hub or Amazon ECR.
- Other suitable storage systems.

A source-code repository and an artifact repository serve different purposes, although a project may use the same platform for more than one purpose.

### Important Practice: Promote the Same Tested Artifact

Where possible, the team should promote the same tested artifact from testing to staging and production instead of rebuilding a potentially different artifact for each environment.

This helps maintain consistency between what was tested and what is released.

---

## 9. Application Environments

An application environment is the infrastructure and configuration in which an application runs.

Different environments support different stages of the software lifecycle.

### Development Environment

Used by developers while making and initially verifying code changes.

### Testing / QA Environment

Used to validate application functionality and detect defects.

### Staging Environment

Used for final checks in an environment designed to resemble production.

### Production Environment

The live environment used by real customers.

### Environment Comparison

| Environment | Main purpose |
|---|---|
| Development | Develop and initially verify code |
| Testing / QA | Validate behavior and detect defects |
| Staging | Perform final pre-release checks |
| Production | Serve real users |

Not every company uses all four environments. The arrangement depends on the application and the team's processes.

---

## 10. What Is Infrastructure?

Infrastructure is the collection of resources and configurations required to run an application.

Infrastructure is more than network settings or access permissions. It includes compute resources, networking, security, storage, scaling, and other supporting services.

### Examples of AWS Infrastructure

#### EC2 — Compute

Amazon EC2 provides virtual servers where applications can run.

**Example:** An EC2 instance hosts a web application.

#### VPC — Networking

A Virtual Private Cloud provides a logically isolated network for AWS resources.

Related components include:

- Subnets
- Route tables
- Internet gateways
- Network connectivity and routing

**Example:** Public-facing resources can be placed in public subnets, while a database can be placed in a private subnet when the architecture requires it.

#### Security Groups — Network Traffic Control

Security groups control allowed inbound and outbound network traffic for associated resources.

**Example:** Allow HTTPS traffic to a web application while restricting administrative access.

#### IAM — Permissions

AWS Identity and Access Management controls what users, roles, and services are authorized to do with AWS resources.

**Example:** A workload can receive a role allowing it to access a specific S3 bucket without granting unrestricted permissions.

**Important distinction:** IAM controls AWS permissions; security groups control network traffic.

#### Elastic Load Balancing — Traffic Distribution

A load balancer distributes incoming application traffic across configured targets.

**Example:** Requests are distributed across multiple healthy application instances.

#### Auto Scaling — Capacity Adjustment

Auto Scaling can add or remove EC2 instances according to configured policies and conditions.

**Example:** The number of application instances increases when demand rises and may decrease when demand falls.

Auto Scaling is not the same as a load balancer. One adjusts capacity; the other distributes traffic.

#### Storage and Databases

Applications may need storage for files and databases for structured application data.

The required services depend on the application architecture.

#### Monitoring

Monitoring helps engineers observe application and infrastructure health, performance, and errors.

For example, Amazon CloudWatch can collect metrics and logs.

---

## 11. Who Creates and Manages Infrastructure?

DevOps engineers, cloud engineers, and platform engineers commonly design, create, configure, and maintain infrastructure. Developers and specialist infrastructure or security teams may also contribute.

The division of responsibility depends on the organization.

### Manual Infrastructure Configuration

An engineer may create resources using the AWS Console or command-line tools.

### Infrastructure as Code (IaC)

Infrastructure as Code defines infrastructure in files that can be version-controlled and applied repeatedly.

Terraform is one example of an IaC tool.

A Terraform configuration can define resources such as:

- VPCs and subnets
- Security groups
- EC2 instances
- Load balancers
- Other supported infrastructure resources

Terraform can then create or update those resources in AWS.

### Why Use IaC?

- Repeatable infrastructure creation
- Easier review of proposed changes
- Version control for infrastructure definitions
- Less repetitive manual configuration
- More consistent environments

---

## 12. Infrastructure Preparation vs. Application Deployment

These are two related but distinct activities.

### Infrastructure Preparation

The team creates or configures the resources needed to run the application.

Examples:

- Creating a VPC and subnets
- Configuring network routes
- Preparing EC2 instances
- Setting permissions and security rules
- Configuring a load balancer

### Application Deployment

The team installs, releases, or starts the application in the prepared environment.

Examples:

- Deploying a JAR file to a server
- Running a Docker image
- Updating an application version

### Does Infrastructure Always Need to Be Created Before CI/CD?

The infrastructure generally needs to exist before the application can be deployed to it. However, it does not necessarily need to be created manually or entirely outside the pipeline.

A CI/CD pipeline can be configured to run infrastructure automation, such as Terraform, before deploying the application.

**Key distinction:** Infrastructure is the place and supporting resources; deployment is the process of releasing the application into that environment.

---

## 13. Deployment Strategies

A deployment strategy defines how a new application version is released.

We covered three strategies: Rolling, Blue-Green, and Canary.

### 13.1 Rolling Deployment

A rolling deployment gradually replaces existing application instances with updated instances rather than updating all instances simultaneously.

#### Example

A website runs on four application instances, all using Version 1.

During deployment:

- The first instance is updated to Version 2.
- Additional instances are updated in batches.
- Old and new versions may run simultaneously during the rollout.
- Eventually, all instances run Version 2.

#### Benefits

- Can reduce downtime.
- Updates occur gradually.
- Does not require replacing every instance at once.

#### Considerations

- Old and new versions must be compatible while they coexist.
- Health checks and traffic management are important.
- Rolling deployment does not guarantee zero downtime.
- Rollback may require reverting updated instances or deploying the previous version.

#### Interview Definition

Rolling Deployment is a release strategy in which application instances are updated gradually in batches. Old and new versions may run simultaneously during the rollout, allowing the application to continue serving users when enough healthy capacity remains available.

### 13.2 Blue-Green Deployment

Blue-Green Deployment maintains two equivalent environments.

- **Blue:** The current live version in our example.
- **Green:** The new version in our example.

The team deploys and tests the new version in Green before switching customer traffic to it.

After validation, traffic is switched to Green. Blue may be retained for rollback.

#### Example

1. Blue serves Version 1.
2. Green is prepared with Version 2.
3. The team tests Version 2 in Green.
4. Traffic is switched to Green.
5. If a problem occurs, traffic may be switched back to Blue, provided rollback is safe.

The color assignments can be reversed in other implementations.

#### Benefits

- The new version can be tested before receiving live traffic.
- Traffic switching can be fast.
- The previous environment may support a quick rollback.

#### Considerations

- Maintaining two environments can cost more.
- Traffic switching is not necessarily instantaneous.
- Database changes and other state changes may complicate rollback.
- Both environments need appropriate configuration and health checks.

#### Interview Definition

Blue-Green Deployment is a release strategy that maintains two equivalent environments. One serves live traffic while the new version is deployed and tested in the other. After validation, traffic is switched to the new environment, and the previous environment may remain available for rollback.

### 13.3 Canary Deployment

Canary Deployment initially releases a new application version to a small percentage of users or traffic.

The team monitors the new version and gradually increases its traffic if it performs well.

#### Example

A shopping application has Version 1 and is releasing Version 2.

- Initially, 95% of traffic goes to Version 1.
- The remaining 5% goes to Version 2.
- The team monitors errors, performance, and health indicators.
- If Version 2 is healthy, more traffic is directed to it.
- Eventually, Version 2 may serve all users.

The percentages are illustrative. Actual values depend on the deployment.

#### Benefits

- Limits initial exposure to the new version.
- Helps detect problems before a full rollout.
- Supports gradual expansion based on observed health.

#### Considerations

- Requires suitable traffic routing and monitoring.
- A small percentage of traffic does not eliminate risk.
- The team needs clear criteria for continuing, pausing, or reversing the rollout.

#### Interview Definition

Canary Deployment is a release strategy in which a new application version is initially exposed to a small subset of users or traffic. The team monitors its behavior and gradually expands the rollout if it performs well.

### 13.4 Comparison of Deployment Strategies

| Feature | Rolling | Blue-Green | Canary |
|---|---|---|---|
| Main approach | Update instances gradually | Switch traffic between two prepared environments | Expose a small portion of traffic to the new version first |
| Old and new versions coexist | Usually during rollout | Both environments can exist before and after the switch | Yes, while traffic is split |
| Main focus | Gradual instance replacement | Environment-level traffic switching | Gradual exposure to users |
| Rollback | May require reverting updated instances | Can be fast if the previous environment remains usable | Stop the rollout or redirect traffic back |
| Key consideration | Compatibility during the rollout | Cost and safe traffic switching | Monitoring and traffic control |

---

## 14. How AWS Services Relate to Deployment Strategies

The AWS services learned earlier can support these strategies, depending on the architecture.

- **EC2:** Hosts application instances.
- **VPC:** Provides the network environment.
- **Security groups:** Control network traffic.
- **IAM:** Provides permissions for users and services.
- **Load Balancer:** Routes traffic to configured targets.
- **Auto Scaling:** Adjusts instance capacity according to configured policies.
- **CloudWatch:** Helps monitor metrics and logs.
- **Terraform:** Can automate infrastructure creation and changes.
- **CI/CD tool:** Automates application build, test, and deployment steps.

These services have different responsibilities. For example, a load balancer distributes traffic, while Auto Scaling adjusts capacity. Neither service automatically implements every deployment strategy without appropriate configuration and orchestration.

---

## 15. End-to-End CI/CD Workflow

Consider a company that maintains an online shopping application.

A typical workflow may be:

1. A developer changes the application code.
2. The changes are pushed to GitHub.
3. A configured trigger starts the pipeline.
4. The pipeline checks out the source code.
5. The application is built.
6. Automated tests run.
7. The application is packaged as a deployable artifact.
8. The artifact is deployed to a testing or staging environment.
9. The team or automated checks validate the release.
10. Required approvals or checks are completed.
11. The application is deployed to production.
12. Monitoring helps the team verify application health.

If a required stage fails, dependent stages normally stop. The team investigates and resolves the problem before proceeding.

The pipeline can also automate infrastructure changes when configured to do so.

This workflow is an example, not a universal template. Some projects use different stages, environments, triggers, or release controls.

---

## 16. Tools and Their Responsibilities

| Technology | Main responsibility |
|---|---|
| Git | Version control |
| GitHub | Hosting source-code repositories and collaborating on code |
| Jenkins | Automating CI/CD workflows |
| Docker | Packaging applications into container images |
| AWS | Providing cloud infrastructure services |
| Terraform | Defining and automating infrastructure creation |
| CloudWatch | Monitoring AWS resources and collecting metrics and logs |

CI/CD is a practice, not a single product. Jenkins is one tool that can implement CI/CD workflows.

---

## 17. Interview Revision Questions and Answers

### Q1. What is CI/CD?

CI/CD is a set of practices that automates code integration, building, testing, and software delivery or deployment.

### Q2. What is the difference between CI and CD?

CI focuses on frequently integrating code and validating it with automated builds and tests. CD extends the process toward delivering or deploying the application.

### Q3. What is the difference between Continuous Delivery and Continuous Deployment?

Continuous Delivery keeps software ready for release, often with a separate production-release decision. Continuous Deployment automatically deploys qualifying changes to production without a manual release approval.

### Q4. What is a pipeline?

A pipeline is a sequence of stages that automates software build, testing, packaging, and delivery or deployment.

### Q5. What is a webhook?

A webhook sends an event notification from one system to another. In CI/CD, a repository platform can notify a pipeline tool when a configured event occurs.

### Q6. What is the difference between Poll SCM and Build Periodically?

Poll SCM checks a repository for changes at configured intervals and can trigger a build when changes are detected. Build Periodically runs at scheduled times regardless of whether code has changed.

### Q7. What is a build artifact?

A build artifact is a deployable output produced by the build or packaging process, such as a JAR file or Docker image.

### Q8. What is a deployment environment?

A deployment environment is the infrastructure and configuration in which an application runs.

### Q9. What is infrastructure?

Infrastructure is the collection of resources and configurations required to run an application, including compute, networking, security, storage, and scaling.

### Q10. Who creates the infrastructure?

DevOps, cloud, and platform engineers commonly create and manage infrastructure, often using automation tools such as Terraform. Responsibilities vary by organization.

### Q11. What is Rolling Deployment?

Rolling Deployment updates application instances gradually in batches rather than updating all instances simultaneously.

### Q12. What is Blue-Green Deployment?

Blue-Green Deployment maintains two equivalent environments and switches traffic to the tested new environment.

### Q13. What is Canary Deployment?

Canary Deployment initially exposes the new version to a small portion of users or traffic, monitors its health, and gradually expands the rollout if it performs well.

### Q14. Does Jenkins create AWS infrastructure automatically?

Not by default. Jenkins can run infrastructure automation tools such as Terraform if the pipeline is configured with the necessary tools, permissions, and steps.

### Q15. What happens if a pipeline stage fails?

The pipeline reports the failure, and dependent stages normally do not proceed. Engineers inspect the logs, investigate the cause, fix the problem, and rerun the pipeline.

---

## 18. Final Learning Summary

The essential CI/CD theory covered in this section includes:

- CI/CD fundamentals and benefits
- Continuous Integration, Continuous Delivery, and Continuous Deployment
- Pipeline stages and workflow
- Triggers: manual, webhook, Poll SCM, scheduled builds, and pull requests
- Checkout, build, test, packaging, and deployment failures
- Source code and build artifacts
- Artifact storage and promotion
- Development, testing, staging, and production environments
- Infrastructure and the responsibilities of DevOps/cloud engineers
- AWS infrastructure concepts: EC2, VPC, IAM, security groups, Load Balancer, Auto Scaling, and CloudWatch
- Infrastructure as Code and Terraform
- Infrastructure preparation versus application deployment
- Rolling, Blue-Green, and Canary Deployment
- The end-to-end code-to-production workflow
- The responsibilities of Git, GitHub, Jenkins, Docker, AWS, and Terraform

**Current status:** Essential CI/CD fundamentals covered at the theory level. Practical work will reinforce the concepts and connect them to real-world DevOps tasks.

**Next topic:** Jenkins fundamentals, followed by guided practical exercises that explicitly connect each exercise to the theory it reinforces.
