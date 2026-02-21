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
  - When the `ci-test.yaml` workflow is triggered, the `docker-compose.test.yaml` file executes frontend and backend tests, storing the test reports as XML files in the `./test_reports/` folder. The `actions/upload-artifact` action uploads these as artifacts, and the `dorny/test-reporter` action uses these test reports to create a custom report attached as a check run to the pull request, or job summary if workflow was triggered manually. This is expanded on [in this workflow explanation](WORKFLOWS_EXPLAINED.md#explaining-ci-testyaml-workflow) and [this actions explanation](CICD_SERVICES_&_TECHNOLOGY_RELATIONS.md#marketplace-actions)

## 3. CREATES appropriate files or commands to automate a deployment process. 15%

**_Uses at least ONE automation workflow file to automate a deployment process at a BASIC level, such as deploying an app to a cloud hosting service._**
**_Meets P criteria, plus uses at least one environment variable appropriately._**
**_Meets C criteria, plus uses environment-specific settings & connections for databases & other services._**
**_Meets D criteria, plus uses the automation workflow to preserve deployment revisions in a cloud hosting service._**

- Deploys app to a cloud hosting service:
  -
- ✅ MULTIPLE environment variables used appropriately
- ✅ Uses environment-specific settings & connections for databases & other services
- ✅ Workflow preserves deployment revisions in cloud hosting service

### 4. CREATES appropriate triggers for an automation workflow. 10%

Uses at least TWO CI/CD workflow triggers OR events at a complex level to initiate an automated workflow, including conditions, schedules, or dependent workflows.

- ✅ `ci-test.yaml` triggers on pull_request
- ✅ `build-and-push.yaml` triggers on version bump
- ✅ `deploy.yaml` triggers on successful completion of `build-and-push.yaml`

### 5. DEVELOPS optimised automation workflow scripts to a professional level. 10%

Uses variables & other reusable code to optimise an automated workflow. Plus uses env variables where appropriate. Plus uses secrets or encrypted keys in an optimal, secure way. Plus creates AT LEAST ONE of the following: writing a custom action/plugin to use within a workflow, export workflow files as downloadable items, export workflow data to reuse in additional workflows.

- ✅ Uses variables & other reusable code to optimise an automated workflow
- ✅ Uses env variables where appropriate.
- ✅ Uses secrets or encrypted keys in an optimal, secure way
- ✅ Exports workflow data to reuse in other workflows (`build-and-push.yaml` uploads tag artifact used by `deploy.yaml`)

### 6. EXPLAINS the purpose & functionalities of an automation workflow. 20%

EXTENSIVE explanation of ALL of the purpose & functionalities of an automation workflow, with diagrams and examples.
_Flowcharts can be from this step, we move to this one. Examples can be with this file we get this result. with this particular PR or branch name we get this result._

- ✅ Explains PURPOSE of all automation workflows
- ✅ Explains FUNCTIONALITY of all automation workflows
- ✅ Uses DIAGRAMS in explanations
- ✅ Uses EXAMPLES in explanations

### 7. EXPLAINS the relations & dependencies of services & technologies involved in an application managed by a CI/CD platform. 20%

EXTENSIVE explanation of ALL of the services & technologies used by an application managed by a CI/CD platform. Plus comparison to alternative software. Plus diagrams and examples included in explanation.
_If you're deploying an express server and it depends on a MongoDB database, you would point out it depends on that database being deployed somewhere, and talk about how your workflow connects those two services together. Like is there a secret you have to pass in to your server? How do you ensure your MongoDB database is available?_
_For a React app, making sure you know the API URL that the app needs to use_

- ✅ Extensive explanation of all services and technologies used by application managed by CI/CD platform
- ✅ Comparison to alternative software
  - _How it might impact your CI/CD workflow_
- ✅ Diagrams and examples included in explanation
