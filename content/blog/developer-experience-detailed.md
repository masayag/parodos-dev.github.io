---
date: 2024-03-15
title: 'Serverless Workflows: an Automated Developer Experience Step-by-Step'
---
In this blog we'll walk you through from a software template to bootstrap the workflow development to having that workflow built, packaged, released and deployed on a cluster.
Please refer to the previous blog for high level explanation and for the architecture of the solution.

## Prerequisites and assumptions
The blog refers to specific set of tools, technologies and methodologies to accomplish its goal.
In high level, we'll start from RHDH (Backstage) by launching the basic workflow template, aiming to work with GitHub as the source control, pushing the workflow image to [Quay](quay.io), using Kustomize to deploy ArgoCD application to apply the GitOps.

* The image is generated and pushed to [Quay](quay.io) and this is not configurable at the moment
* The target namespace of both the pipeline and the workflow is not configurable and equal to `sonataflow-infra`

### Create workflow repository
In this walkthrough we'll create a workflow named demo. We'll use Quay organization named `orchestrator-testing`.
As a first step we should create a repository on Quay to store the workflow image. We'll use `orchestrator-testing/serverless-workflow-demo`.

![new workflow repository in Quay](/blog/images/new-quay-workflow-repo.png)

### Add robot account permissions
Add robot account permissions to the created repository

![permissions](/blog/images/add-robot-accout-perm-to-workflow.png)


### Create a secret for GitOps cluster
 The instructions for creating the secret and configuring it for the target cluster are detailed [here](https://github.com/parodos-dev/orchestrator-helm-chart/blob/gh-pages/gitops/README.md#installing-docker-credentials).


## Creating the software template
The Orchestrator plugin comes with its own templates designed to kickstart your workflow project. By selecting a template tagged with `orchestrator`, you gain access to the following benefits, all at no cost:

* A fully operational software project to develop your serverless workflow, in a newly generated Git repository under the organization of your choice.
* A ready-to-use configuration repository with a [kustomize](https://kustomize.io/) configuration to deploy the workflow on the designated RHDH instance.
* (*) Automated CI tool deployment to build workflows on the selected cluster using OpenShift Pipelines.
* (*) Automated CD automation deployment to deploy applications implementing your workflow using OpenShift GitOps.

### Selecting the template
Navigate to the Catalog and select the `Basic workflow bootstrap project` template. Select the template and click on "Launch Template" to start populating the input parameters designed for creating the target GitHub repositories to represent the workflow and its GitOps projects.

![software-template](/blog/images/software-template-catalog.png)

### Launching the template
There are several screens for completing the workflow creation from a template. Let's review the parameters of the first screen (the rest of the parameters will be described here):

![input parameters](/blog/images/template-input-parameters-1.png)

- **Organization Name** - The GitHub organization name in which the workflow repositories will be created. The GitHub token provided when the Orchestrator chart was installed is expected to include permissions to create repositories in this organization.
- **Repository Name** - The name of the repository to include the workflow definition, the spec and schema files and the application properties. The development of the workflow happens in this repository. Assuming this repository is named `onboarding`, a second repository named `onboarding-gitops` is created to provide the CD automated deployment of the workflow.
- **Description** - The description will be added to the README.md file of the generated project and also for the workflow definition that is shown in the Orchestrator plugin
- **Workflow ID** - A unique identified of the workflow. The ID is used to generate the resources in the project (appears as part of the file names) and it also act as the name of the Sonataflow CR for that workflow. After that CR is deployed to the cluster, the ID identifies the workflow in Sonataflow.
- **Workflow Type** - There are two types of supported workflow. An *infrastructure* workflow, which is simply a workflow to perform some operation and returns its output. The other type is the *assessment* workflow that is used to perform an evaluation or assessment and based on its result to offer potentials infrastructure workflows to the user.
- **Select a CI/CD method** - If selected *None*, no GitOps resources will be created in the target repositories. In fact, only the workflows source repository will be created. For *Tekton with ArgoCD* option, two repositories will be created: one containing the workflow and a second repository for the GitOps resources to deploy the built workflow on a cluster.
- **Workflow Namespace** - The namespace of the workflow to be deployed in the target cluster. Due to current limitations, the only supported namespace for now is *sonataflow-infra* in which Sonataflow infrastructure is deployed.
- **GitOps Namespace** - The namespace in which the GitOps secrets were created and ArgoCD application will be created.
- **Quay Organization Name** - The Quay Organization Name of the published workflow. The Tekton pipeline to build the workflow will push to that organization.
- **Quay Repository Name** - The Quay Repository Name of the published workflow. The repository must exist before deploying the GitOps.The secret created in the *GitOps Namespace* must have permission to push to that repository.
- **Enable Persistance** - To enable persistence for workflow, check this option. This will make sure each workflow will persist its instances on a schema in the database as configured. The schema name will be the same as the workflow ID. Using persistence for workflows is recommended for avoid lose of information for long running workflows and to support the *Abort* operation.
- **Database properties** - The list of properties is self explained.

Once all of the parameters are provided, click on *Review* and make sure they are correct, then click on create.
If all went well, you should expect to see:

![template created](/blog/images/template-created.png)

There are links to three resources:
- **Bootstrap the GitOps Resources** - This points directly to the workflow GitOps repository. In a single step, the GitOps for the ArgoCD can be deployed to the target cluster. We'll review this step shortly.
- **Open the Source Code Repository** - The Git repository in which the workflow development should happen.
- **Open the Catalog Info Component** - The RHDH Catalog Components view which should include the newly created components: the workflow source repository and the workflow GitOps repository.

## Bootstrap the GitOps Resources

## Open the Source Code Repository

## Open the Catalog Info Component

### View the Source Code Repository Component

### View the GitOps Resources Repository Component

### Ready for Production
Get ready for another quick section.

Once the software has been validated and released, the CI/CD pipelines are triggered again to build and deploy the application in the production environment. Easy-peasy, and once again, making efficient use of the developer's time.

## Wrapping Up
What are you waiting for then? Design your first workflow and let the Orchestrator handle the tedious tasks for you. 

Get customer-ready in just a minute with the power of the Automated Developer Experience for RHDH Orchestrator!


