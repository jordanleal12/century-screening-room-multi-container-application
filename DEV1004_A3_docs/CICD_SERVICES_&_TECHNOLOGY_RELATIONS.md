# Explaining the Relations & Dependencies of Services & Technologies Managed by CI/CD Platform

The following is an explanation of all services and technologies that are used, configured or managed by the CI/CD automation workflows of this application, including the CI/CD automation platform itself.

## GitHub Actions

GitHub Actions acts as the orchestrator for CI/CD automation for this application, managing the entire lifecycle from the initial code change, to the final deployment. In doing so, this application reduces human error, enforces consistent and safe code integration, and increases the speed and efficiency of creating new iterations of the application.

This process is achieved through the implementation of three workflow files, which are [explained in greater detail here](WORKFLOWS_EXPLAINED.md), and expanded on briefly below:

1. `ci-test.yaml`: This workflow automates testing of the backend and frontend services before they can be merged to the main branch, attaching persistent reports to the pull request as check runs.
2. `build-and-push.yaml`: This workflow builds a container image of the frontend and backend service, pushing it to the AWS Elastic Container Registry (ECR). This workflow is triggered manually or through a Git version update, with the container images being semantically tagged depending on their current version.
3. `deploy.yaml`: This workflow automates deployment of the application to AWS Elastic Container Service (ECS), using the previously built container images.

An overview of the services and technologies utilized within the workflows:

```mermaid
---
config:
  theme:  dark
---
flowchart LR
    subgraph ci-test["CI Test Workflow"]
        direction TB
        A[Docker Compose builds and runs backend & frontend service test containers] -->
        B[Upload-artifact action uploads results as artifact] -->
        C[Test-reporter action attaches results as 'check run']
    end
    subgraph build-and-push["Build and Push Workflow"]
        direction TB
        D[Checkout action checks out repository code] -->
        E[Configure-aws-credentials configures job credentials] -->
        F[Amazon-ecr-login logs the docker client into AWS ECR]
        G[Image tags created then uploaded with upload-artifact action] -->
        H[Docker is used to build and push service image to ECR]
    end
    subgraph deploy["Deploy Workflow"]
        direction TB
        I[Configure-aws-credentials configures job credentials] -->
        J[Download-artifact action downloads tag artifact] -->
        K[Amazon-ecs-deploy-express-service deploys both services to ECS]
    end
    ci-test -- passes & version bump --> build-and-push
    build-and-push -- completes successfully --> deploy
```

### Why GitHub Actions?

There are multiple automation platforms available to developers, but the one used by this application is GitHub Actions, which makes use of yaml workflow files to execute instructions in 'runners' (a virtual machine instance). It also has an impressive suite of predefined actions available in the GitHub Marketplace, allowing developers to abstract many automation jobs. [According to a 2025 Jetbrains Survey](https://blog.jetbrains.com/teamcity/2025/10/the-state-of-cicd/), GitHub Actions leads in popularity as the CI/CD tool of choice, with the next three being GitLab CI, Jenkins and Azure DevOps Server respectively. The chart below shows a comparison of the options, with each comparison to be expanded on:

|                             | **GitHub Actions**  |    **GitLabs CI**     |     **Jenkins**     |    **Azure DevOps**     |
| :-------------------------: | :-----------------: | :-------------------: | :-----------------: | :---------------------: |
|  **Control Plane Hosting**  |        SaaS         |  SaaS or Self Hosted  |     Self Hosted     |   SaaS or Self Hosted   |
|     **Runner Hosting**      | SaaS or Self Hosted |  SaaS or Self Hosted  |     Self Hosted     |   SaaS or Self Hosted   |
|     **Learning Curve**      |         Low         |        Medium         |      Very High      |          High           |
| **GitHub Repo Integration** |      Simplest       |        Complex        |    Most Complex     |         Simple          |
|  **Cost With Public Repo**  |        Free         | Free, Limited Runners | Infrastructure Cost |     Licencing Cost      |
|  **Ecosystem Integration**  | Actions Marketplace |    CI/CD Catalogue    |   Jenkins Plugins   | Azure Devops Extensions |

**Control Plane Hosting:** This refers to the hosting of the 'control plane' of the CI/CD tool, being the hardware and software orchestrating the execution of the workflows. This is separate to the 'runner' virtual machine instances that execute specific workflow tasks, with the control plane handling the management of these runners. For the scope of our application, a CI/CD tool offering this as cloud hosted Software as a Service (SaaS) significantly reduces initial setup and ongoing maintenance complexity. Of the 4, only Jenkins does not offer the control plane as SaaS, although some third party providers do offer this as a service.
_**Result:**_ GitHub Actions: 1 | GitLabs CI: 1 | Jenkins: 0 | Azure Devops: 1

**Runner Hosting:** As mentioned above, a runner is a virtual machine instance that executes workflow tasks within. For the same reason as the control plane hosting, having cloud hosted SaaS runners for our application is desired. Again, Jenkins is the only one which does not offer this.
_**Result:**_ GitHub Actions: 2 | GitLabs CI: 2 | Jenkins: 0 | Azure Devops: 2

**Learning Curve:** This is a comparison of both the base learning curve of these tools, adjusted to consider my familiarity with GitHub as a platform. GitHub Actions is widely considered the easiest learning curve, especially for someone already familiar with the platform. It features an extremely comprehensive suite of documentation, intuitive UI, and extensive community support. On the opposite end of the scale, Jenkins has a significantly steep learning curve, requiring a new user to manage both the hardware and software themselves. Jenkins offers extreme customization, at the expense of ease of use. In second place, GitLabs CI is considered a close second to GitHub Action, but its use of single workflow files for an entire workflow and a less clean workflow syntax keeps it a step behind in learning curve
_**Result:**_ GitHub Actions: 3 | GitLabs CI: 2 | Jenkins: 0 | Azure Devops: 2

**GitHub Repository Integration** This is a slightly unfair but a relevant comparison, as this application already existed on a GitHub Repository. As expected GitHub Actions easily wins here, requiring a simple commit of a workflow to `.github/workflows/` in the repository root. Authentication is also extremely easy to manage with a GitHub Token. GitLabs CI and Azure Devops both require mirroring of the existing repository, and Jenkins requires the complex manual configuration of webhooks.
_**Result:**_ GitHub Actions: 4 | GitLabs CI: 2 | Jenkins: 0 | Azure Devops: 2

**Cost with Public Repository:** Since there are different cost options for public vs private repo's with this application being a public repository, only the cost with public repo's will be compared. Both GitHub Actions and GitLabs CI offer free usage of their control plane service, and free usage of their cloud hosted runners. However, GitLabs CI caps this usage at 400 compute minutes and 10GiB of storage per month. Neither Jenkins nor Azure Devops are able to be used for free, with Azure Devops requiring licencing fees, and Jenkins incurring infrastructure costs for self hosting.
_**Result:**_ GitHub Actions: 5 | GitLabs CI: 2 | Jenkins: 0 | Azure Devops: 2

**Ecosystem Integration:** This refers to the different tools, plugins, templates etc. available to integrate within workflows, offered by the CI/CD tools. Here, GitHub Actions stands ahead with its rapidly growing Actions Marketplace. This ecosystem is open source and hosts over 20,000 actions already, including official actions from verified third party providers like AWS and Google. GitLabs CI's CI/CD catalogue differs by offering reusable components and script templates rather than third party scripts. Azure Devops Extensions Marketplace is more focused on integration across Microsoft services, with a smaller range. Jenkins plugins offer significant customization, but are more complex to manage and fall well short of the amount of actions available on the Actions Marketplace
_**Result:**_ GitHub Actions: 6 | GitLabs CI: 2 | Jenkins: 0 | Azure Devops: 2

**Overall Result:** GitHub Actions stands as the clear leader, winning or tying for every category for my specific use case.

---
