# Rubric Requirements

This file outlines the HD rubric requirements for DEV1004 Assessment 3, explaining where/how this repository attempted to meet these requirements.

## 1. DEVELOPS semantically and syntactically valid & complete automation workflow files. 10%

**_Implements at least ONE automation workflow file(s) OR help file(s) which are COMPLETELY semantically & syntactically valid._**

- THREE workflows implemented:
  - [`ci-test.yaml`](../.github/workflows/ci-test.yaml)
  - [`build-and-push.yaml`](../.github/workflows/build-and-push.yaml)
  - [`deploy.yaml`](../.github/workflows/deploy.yaml)

## 2. CREATES appropriate files or commands to automate a testing process. 15%

**_Uses at least ONE automation workflow file to automate a testing process at a DETAILED level, such as building a custom log file out of the testing results._**
**_Meets D criteria, plus uses the automation workflow to store logs in a persistent location._**

- At least ONE automation workflow file to automate testing:
  - [`ci-test.yaml`](../.github/workflows/ci-test.yaml)
- Custom log file built from test results and workflow stores logs in persistent location:
  - When the `ci-test.yaml` workflow is triggered, the `docker-compose.test.yaml` file executes frontend and backend tests, storing the test reports as XML files in the `./test_reports/` folder. The `actions/upload-artifact` action uploads these as artifacts, and the `dorny/test-reporter` action uses these test reports to create a custom report attached as a check run to the pull request, or job summary if workflow was triggered manually. This is expanded on [in this workflow explanation](./WORKFLOWS_EXPLAINED.md#explaining-ci-testyaml-workflow) and [this actions explanation](./CICD_SERVICES_&_TECHNOLOGY_RELATIONS.md#marketplace-actions)

## 3. CREATES appropriate files or commands to automate a deployment process. 15%

**_Uses at least ONE automation workflow file to automate a deployment process at a BASIC level, such as deploying an app to a cloud hosting service._**
**_Meets P criteria, plus uses at least one environment variable appropriately._**
**_Meets C criteria, plus uses environment-specific settings & connections for databases & other services._**
**_Meets D criteria, plus uses the automation workflow to preserve deployment revisions in a cloud hosting service._**

- Workflow deploys app to a cloud hosting service:
  - [Proof of deployment shown here](./PROOF_OF_DEPLOYMENT.md), with the workflow process explained [in this workflow explanation](./WORKFLOWS_EXPLAINED.md#explaining-deployyaml-workflow)
- Uses environment variables appropriately:
  - [This section](./CICD_SERVICES_&_TECHNOLOGY_RELATIONS.md#secret-management) explains and shows secret management in the workflow.
- Uses environment-specific settings & connections for databases & other services:
  - The [workflows explained](./WORKFLOWS_EXPLAINED.md) file explains each environment specific setting and connection used in each workflow file.
- Workflow preserves deployment revisions in cloud hosting service:
  - This [proof of deployment section](./PROOF_OF_DEPLOYMENT.md#images-stored-and-pulled-from-ecr) shows multiple deployment revisions stored in ECR.

## 4. CREATES appropriate triggers for an automation workflow. 10%

**_Uses at least TWO continuous integration/continuous delivery (CI/CD) workflow triggers or events at a complex level to initiate an automated workflow, including conditions, schedules, or dependent workflows._**

- Uses at least 2 CI/D workflow triggers/events to initiate workflow:
  - `ci-test.yaml` triggers on pull_request
  - `build-and-push.yaml` triggers on version bump
  - `deploy.yaml` triggers on successful completion of `build-and-push.yaml`

### 5. DEVELOPS optimised automation workflow scripts to a professional level. 10%

**_Uses variables & other reusable code to optimise an automated workflow._**
**_Meets P criteria, plus uses environment variables where appropriate._**
**_Meets C criteria, plus uses secrets or encrypted keys in an optimal, secure way._**
**_Meets D criteria, plus creates functionality to cover AT LEAST ONE of the following: writing a custom action/plugin to use within a workflow, export workflow files as downloadable items, export workflow data to reuse in additional workflows._**

- Uses variables & other reusable code to optimise an automated workflow:
  - The [workflows explained](./WORKFLOWS_EXPLAINED.md) file explains where variables and reusable code are used to optimise each workflow file.
- Uses env variables where appropriate:
  - Also explained in the above mentioned workflow
- Uses secrets or encrypted keys in an optimal, secure way:
  - Explained in the [workflows explained](./WORKFLOWS_EXPLAINED.md) file and [secret management section](./CICD_SERVICES_&_TECHNOLOGY_RELATIONS.md#secret-management)
- Exports workflow data to reuse in other workflows:
  - `build-and-push.yaml` uploads tag artifact used by `deploy.yaml`

### 6. EXPLAINS the purpose & functionalities of an automation workflow. 20%

**_EXTENSIVE explanation of ALL of the purpose & functionalities of an automation workflow, with diagrams and examples._**

- Explains purpose and functionality of all automation workflows:
  - Explained in the [workflows explained](./WORKFLOWS_EXPLAINED.md) file
- Uses diagrams and examples in explanations:
  - Above mentioned file contains diagrams and examples, with additional examples provided in the [proof of deployment](./PROOF_OF_DEPLOYMENT.md) file.

### 7. EXPLAINS the relations & dependencies of services & technologies involved in an application managed by a CI/CD platform. 20%

**_EXTENSIVE explanation of ALL of the services & technologies used by an application managed by a continuous integration/continuous delivery (CI/CD), comparison to alternative software, and with the explanation including diagrams and examples._**

- Extensive explanation of all services and technologies used by application managed by CI/CD platform:
  - [This CI/CD services and technology relations file](CICD_SERVICES_&_TECHNOLOGY_RELATIONS.md) contains the required extensive explanations
- Diagrams and examples included in explanation:
  - The above mentioned file includes diagrams and examples.
