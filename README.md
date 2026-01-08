🚀 Project 1: Jenkins CI/CD Pipeline with Controlled Deployments
📌 Project Overview

This project implements a CI/CD pipeline that automates build and validation of application artifacts, and performs controlled deployment to a web server only when a valid artifact is produced. The pipeline separates CI and CD stages and introduces human-controlled parameters to ensure safe deployments.

🏗️ CI/CD Architecture

Source code is stored in GitHub and changes trigger Jenkins via a webhook. Jenkins runs the CI pipeline to build, test, and produce a validated artifact. The artifact is automatically deployed to the dev environment, while production deployments require an explicit prod_confirm parameter to ensure controlled releases.

🧰 Tools Used

Jenkins (CI/CD orchestration)

GitHub (source code management)

Linux (execution environment)

EC2 (Jenkins and web server host)

Shell scripting (automation)

🔄 Pipeline Workflow
Checkout

The checkout stage pulls a specific and reproducible version of the source code into the Jenkins agent workspace, ensuring all subsequent stages operate on the same codebase.

Build

The build stage packages the application into a deployable form and produces a consistent output. This separation ensures only stable builds are promoted further in the pipeline.

Test

The test stage validates the built artifact and enforces a fail-fast mechanism. Any test failure stops the pipeline to prevent unstable artifacts from reaching deployment environments.

Artifact

The artifact stage stores a versioned and immutable build output, decoupling build from deployment. This enables reliable promotion across environments and quick rollback to a known-good version.

Deploy

The deploy stage releases the artifact to target environments. Deployments to dev and stage are automated, while production deployments are guarded by explicit parameters to ensure human-controlled and safe releases.

⚙️ Pipeline Parameters

env – Target environment (dev, stage, prod)

version – Artifact version to deploy

prod_confirm – Boolean confirmation required for production deployment

🚨 Failure Scenarios & Handling
Test Failure

If the test stage fails, the pipeline stops immediately and is marked as failed. No artifact is created and no deployment occurs, ensuring unstable builds are not promoted.

Artifact Missing or Invalid

If the artifact is missing or incorrectly referenced, the deployment stage does not run. Operators should verify artifact generation, naming, and versioning before re-running the pipeline.

Deployment Permission Failure

If deployment fails due to permission or access issues, the pipeline fails at the deploy stage while the artifact remains valid. Operators should investigate environment-level permissions and access controls.

Production Confirmation Missing

If prod_confirm is not set to true, the production deployment stage is intentionally skipped. The pipeline succeeds without deploying, preventing accidental production releases.

🔁 Rollback Strategy

The pipeline maintains versioned and stable artifacts, enabling rollback to a previously known-good version if a deployment issue occurs. In case of production failure, operators can redeploy the last successful artifact without rebuilding, minimizing downtime and risk.

▶️ How to Run / Trigger

Pipeline can be triggered automatically via GitHub webhook on code push.

It can also be triggered manually from Jenkins with required parameters.

Production deployments require explicit confirmation.

🚀 Improvements & Future Enhancements

Add automated notifications (Slack / Email)

Integrate Docker image builds

Add infrastructure provisioning using Terraform

Introduce automated security scans

Extend pipeline to Kubernetes deployments
