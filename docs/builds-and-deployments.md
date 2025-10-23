# Builds and Deployments

|                    |                 |
| ------------------ | --------------- |
| **Content Owners** | @Owner |
| **Last Updated**   | 2025-10-01      |

---

## Containers are Central to our CI Build Pipeline

---

Using Docker and Docker Compose for our build process does a very special thing for us: It makes the build environment predictable and uniform. That means the build we run locally is the same build that runs within our CI pipeline. No more "it works on my machine" nonsense.

This may seem obvious for services that are hosted in a container, but it holds even for things like command-line tools that are installed locally.

For services and processes that are natural fits for hosting in a container, we use a multi-stage Docker build. For a locally installed tool, we don't need a multi-stage build; we just copy the compiled binary out of the build container.

## Basic Build Architecture

---

All CI builds will have four basic parts that come together to make it all work:

1. **The CI build definition**: This can be a definition in Azure DevOps, AWS, GitHub, etc. The pipeline technology really doesn't matter.
2. **A Dockerfile**: This executes the actual compilation, and if required, creates the final image.
3. **End-to-End Test suite**: These are the end-to-end tests, a.k.a. `e2e` tests, that perform testing as if it were a user of the tool, or a client of the service.
4. **An orchestrator script**: This POSIX-compliant script is the "glue" that brings it all together. We typically call the file `build.sh`. This file will make sure all necessary tooling is installed into the build environment, run linting, code analysis, the unit tests, and then call `docker build...`. Assuming all the steps pass, it will then execute the end-to-end tests.

> **Example Projects**
>
> **Service example:** A good example of a build that produces a Docker image that contains the service or process can be found in this project: <https://company.visualstudio.com/Team/_git/project>. **_All examples below will come directly from this project._**
>
> **Tool example**: A good example of how a build uses Docker to create a tool that is installed locally, look at this project: <https://company.visualstudio.com/Team/_git/project?path=/tests&version=GBmain&_a=contents>.

### The CI Build Definition

This file is typically a `yaml` file that defines the build for the pipeline. It will have sections where you'll configure the OS (we use Linux), pull down the code, set up any authentication contexts, and after the build script (the orchestrator script) has completed, publish any artifacts to wherever they may need to go, such as test results, Docker Images, code artifacts, etc. The details of what actually gets published from project to project may change, but the responsibility of the yaml build definitions is the same. Here is an example from the SAMPLE MGMT API project:

### The Dockerfile

The Dockerfile is core to the build. Within it, the actual binary is compiled within an environment that is 100% under your control. The benefit is that it will build anywhere since there are no external dependencies. Everything it needs to build is within the Docker container. Compilation tools? There. Supporting tools like code generators, linters, and analyzers? There. The build experience is the same regardless of where it is run!

Here is an example of a multi-stage build from the SAMPLE MGMT API project:

### The Orchestrator Script

This file, as mentioned before, is the glue that pulls it all together. The basic steps are orchestrated by the `build.sh` scripts are:

1. Ensure the necessary authorization is granted.
2. Install any required tools the script will need to orchestrate the build.
3. Run any linting, code analysis, and unit tests (in that order).
4. Build the Docker image.
5. Run the end-to-end tests.
6. Prepare any files for any further action that may need to be managed by the CI pipeline. These are usually things like pushing the Docker image to a private repository, publishing analysis results, etc.

Here is an example of a multi-stage build from the SAMPLE MGMT API project. The functions are named in such a way that they describe what each step in the build process is.

### End-to-End Test Suite

> **IMPORTANT!**
>
> Developing solid end-to-end tests will probably take just as long as implementing the solution you're developing. So, keep this in mind. You need to factor this in when starting a new project or adding functionality to an existing project.

This is a little more complex to dive into, so we won't go into the details too far. For a look at what end-to-end tests look like in detail, please refer to the end-to-end test for the SAMPLE MGMT API: <https://company.visualstudio.com/Team/_git/project?path=/tests>

These tests are written as consumers of the API and doesn't rely upon any of the code from the API project. It only relies upon what a client may have access to, such as API documentation like swagger.json. In the case of SAMPLE MGMT API, `BATS` and `curl` are the core tools to write the tests, and `SQLCMD` is used to validate any database changes that should be expected. Responses are evaluated against what is documented in the OpenAPI specification (swagger.json).

When writing the end to end tests the tools or language to write the tests are secondary. As long as the result is that the tests are conducted from a consumer's perspective, based on the documentation the caller would have, it is fine.

The typical structure of the end-to-end tests are like so, contained within a directory named [tests](https://company.visualstudio.com/Team/_git/project?path=/tests) at the project's root.

- [.conf](https://company.visualstudio.com/Team/_git/project?path=/tests/.conf) directory: This contains any necessary files, like configurations, certs for testing, etc., that the testing infrastructure will need. In this example, we needed a cert for the OTEL collector, and a configuration for it as well.
- [bats](https://company.visualstudio.com/Team/_git/project?path=/tests/bats) directory: This contains the actual tests. Your tests may be in a different directory. I use BATS because it doesn't require any compile steps; it just needs to be copied over to the docker image. This keeps the Dockerfile that is build to execute the tests fairly simple.
- [Dockerfile](https://company.visualstudio.com/Team/_git/project?path=/tests/Dockerfile): This builds a docker image containing the tests and any runner scripts needed. When a container is created using the image, the testing starts.
- [docker-compose.yaml](https://company.visualstudio.com/Team/_git/project?path=/tests/docker-compose.yaml): This sets up the infrastructure the tests will needs. In the case of SAMPLE MGMT API, we need to set up the following:
  - `otel_collector`: This is so that traces from the SAMPLE MGMT API will have a working endpoint, and not create any unnecessary errors.
  - `project_databases`: This is a custom image that we use that contain the database accessed by SAMPLE MGMT API.
  - `project_api`: This will be the image that was created during the build.
  - `m_tests`: This is the image that contains the tests that will be executed.

## Deployments

---

We value and strive to be able to perform zero-downtime deployments. We use release pipelines (currently in Azure DevOps) to help manage this. Continuing with using SAMPLE MGMT API as an example, here is it's pipeline definition: [https://company.visualstudio.com/Platform Engineering/\_releaseDefinition?definitionId=4&\_a=definition-pipeline](https://company.visualstudio.com/Team/_releaseDefinition?definitionId=4&_a=definition-pipeline).

The one thing we DO NOT automate is deployment; a human needs to "push the button". We want the be very explicit about when a deployment takes place. This is so we can monitor the deployment to make sure we can rollback quickly if needed.

### Hosting

You can't talk about deployments without talking briefly about hosting. Our ideal is Kubernetes. Since we're on Azure, that means [AKS](https://azure.microsoft.com/en-us/products/kubernetes-service). A acceptable fallback is [Azure Container Apps](https://azure.microsoft.com/en-us/products/container-apps). We do NOT want to use virtual machines, or even Azure App Services. We do have some older software deployed on these platforms, but we are actively working towards on transitioning them to use these better hosting options.

## Revision History

---

- 2025-10-02: @Owner - Initial Release
