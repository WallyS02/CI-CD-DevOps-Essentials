# CI-CD-DevOps-Essentials
## If I'll find any other feature to be useful - it'll surely be added!
## How it works?
CI/CD is essentially automating processes between dev and ops teams.
* Continuous Integration \(CI\) focuses on frequent code changes merges to its main version \(main/master branch\), detecting bugs by building code and running automated tests \(on this stage usually unit, integration tests and linting with SAST\). Code is integrated faster than manually and everything that can be automated is automated.
* CD can be interpreted as Continuous Delivery or Continuous Deployment. Continuous Delivery means that end result of pipeline is a production-ready, built and fully tested \(in different environments\) code that can be deployed anytime, e.g. after manual confirmation. Continuous Deployment extends Continuous Delivery by involving automatic deployment to production environment.

CI/CD practises shorten integrations, deliveries or deployments from really long time to even minutes if designed and implemented correctly. Whole process is consistent and closely monitored, problems and bugs are detected earlier by shifting testing to the left.
## Pipeline architecture
Different pipelines are made for different projects and consist of different stages but some usual are presented below.
### Linting and SAST
Linting is static code analysis without executing it. It helps in detecting syntax errors and enforcing code standards to prevent integrating malfunctioning code and maintaining its quality. Linting also can detect security problems that are critical in running code in production.

Linting is usually the first stage at pipeline - e.g. before committing - that stops it if code does not meet the requirements. Thanks to this problems are detected early and it allows developers to focus on code logic during code reviews. Example of linting tool is SonarQube that works with multiple languages and also does some SAST.

SAST is another static code analysis but focuses on detecting security problems with analysed code. Like with SonarQube, it can be done together with linting by one tool.
### Dependencies
Dependencies are all of external libraries, tools, frameworks, services - basically code that is not maintained by the team but used in project. Like in code in project, it can contain vulnerabilities and suddenly change so it should be managed responsibly.

Dependencies are managed by package managers \(e.g. npm, pip, Maven\) to ease maintaining usage of all of them. Versions of dependencies are managed and changed cautiously if needed - they can differ in a bad way.

It is important to scan dependencies for vulnerabilities with tools like Dependabot to be aware of security problems found in versions that are used in a project. These tools also help with upgrading to versions that fix these vulnerabilities so no need for searching by yourself.

According to semantic versioning \(MAJOR.MINOR.PATCH\) minor and patch versions can be upgraded safely automatically, because they are compatible backwards. Major changes should be upgraded manually because they can make our project stop working correctly.

In pipeline dependencies should be provided before building or testing application.
### Building
Building is compiling or transpiling source code into ready-to-deploy artifacts. It is done by using compilers or transpilers designed for specific language. Compilers or transpilers are usually included in package managers so there is no need for using other ones. Before building unit and integration tests should be conducted if prepared. When artifact is ready it can be published in artifact repository or used in containerizing the application.

During build it is best to freeze dependencies to prevent them from unpredictable upgrades that can mess up build. Build can help detecting errors so its another layer of maintaining quality of the project.
### Testing
Testing is \(usually\) automatic code verification on multiple stages of pipeline. It detects bugs on early stages of application lifecycle and assures its stability at production environment. Tests divide on static and dynamic - static tests were described earlier \(Linting and SAST\).

Tests can be executed really slow so some of them can be conducted in parallel to speed up their execution.

Tests efficiency in pipeline can be measured by different metrics, e.g. code coverage, average test duration or error rate.

In typical pipeline unit and integration tests are executed before building application and when it is ready to be launched it is tested in testing environment before deploying it to staging or production environment.
#### Unit Tests
Unit tests are earliest-executed tests. They verify single components in isolation. For different languages and frameworks there are different tools for unit testing \(JUnit - Java, Vitest - Vite, Jest - JavaScript\).
#### Integration Tests
Integration tests are conducted after unit tests. They verify cooperation of different components and modules. Usually same tools as in unit testing can be used for integration testing.
#### End-To-End \(E2E\) Tests
E2E tests are simulation of application user behaviour in environment that is as close as possible to production. They are conducted in separate testing environment where application prepared in earlier stages is launched. This tests are executed slowly so it is a good idea to parallelize them. Usual tools examples that are used for this type of testing are Selenium, Cypress and Playwright.
#### Load / Performance Tests
Load / performance tests measure applications efficiency, scalability, resource usage, response time and other metrics under high workloads. Like in E2E tests, separate environment is needed to ensure tests credibility. Usual tools examples that are used are JMeter, Locust, k6.
#### DAST
DAST is scanning running application to find security vulnerabilities. Like in E2E tests, separate environment is needed to ensure tests credibility. Popular tools example is OWASP ZAP.
### Containerizing
Containerizing is packing application with its dependencies into isolated environment called container. You can find more about it in [Docker DevOps Essentials](https://github.com/WallyS02/Docker-DevOps-Essentials). In pipeline images can be built using application prepared in earlier stages and pushed to image registry where ready to use application images are stored. 
### Deploying
Deploying is a process of transferring application to production environment in controlled and safe manner, usually from staging environment. After deploy application should be closely monitored to detect any problems. When something goes wrong previous application version should be rollbacked.
#### Strategies
There are some strategies of deploying applications to prevent downtime during deployment. Popular examples are:
* **Blue/Green Deployment** - creating 2 identical environments \(Blue and Green\) that differ in application version that is launched inside each of them. After testing new version of the application traffic is directed to environment with new application version.
* **Canary Deployment** - 2 environments are created like in Blue/Green Deployment and traffic is being gradually redirected to environment with new version observing if errors occur - if not environment with new version ends up with 100% of traffic.
* **Rolling Update** - instances of application \(e.g. containers\) with old version are replaced with new ones that contain new application version. This is default strategy for Kubernetes.
## GitHub Actions
Github Actions is a CI/CD platform integrated with GitHub. Pipelines are defined in YAML code and integration allows for reacting to repository events \(push, PR, issue\).

Pipelines are executed by runners which can be GitHub-hosted or self-hosted. GitHub-hosted runners are managed by GitHub where OS of machine can be chosen \(Ubuntu, Windows, macOS\) and self-hosted are self-managed servers.

Pipelines can be triggered by different events:
* **repository events** - push, pull_request, release, issue_comment, etc. - check in documentation
* **schedule** - pipelines are triggered with defined schedule, e.g. every day night tests
* **workflow_dispatch** - manual pipeline trigger

There is a lot of ready-made actions that can be used in your pipelines - just look in GitHub Marketplace.

You can use secrets in your pipeline - define one in ```Settings -> Secrets and variables -> Actions``` and use it in code with ```${{ secrets.<secret_name> }}```.

To test single job in different environments use matrix strategies that execute multiple, parallel job runs with different environment parameters.
Example:
```
jobs:
  <matrix_name>:
    strategy:
      matrix:
        <version_variable>: [<version>]
        os: [<platform>]
```

To run service containers in pipeline use job services.
Example:
```
services:
  <service_name>:
    image: <service_container_image>
    ports:
      - <host_port>:<container_port>
```

To cache dependencies to improve pipeline execution time use ```actions/cache``` tailored to the project.

To test pipelines locally use [act](https://github.com/nektos/act?tab=readme-ov-file).

To define pipeline create ```.github/workflows/``` directory in projects root folder and create pipeline's YAML file there.

Example pipeline YAML:
```
name: <name>  
on:  
  <trigger>:  
    branches: [<branch_name>]  
  <trigger>:  
    branches: [<branch_name>]  

jobs:  
  <job_name>:  
    needs: <other_job_name>  
    if: <condition>  
    runs-on: <platform>  
    steps:  
      - name: <name>  
        uses: <action>
        run: <command>
        working-directory: <working_directory>
        with:
          <variable_name>: <variable_value>
```
Pipeline YAML file consists of:
* **name** - pipeline name
* **on** - when pipeline is triggered by repository event and which branches it concerns
* **jobs** - stages of pipeline
  * **needs** - selects jobs that must be completed before running this job
  * **if** - prevents job from running unless defined condition is met
  * **runs-on** - defines the type of machine to run the job on
  * **step** - single task in job
    * **uses** - selects an action to run as part of a step in your job
    * **run** - runs commands in cli
    * **working-directory** - specifies the working directory of where to run the command
    * **with** - a map of the input parameters defined by the action
## GitLab CI/CD
GitLab CI/CD is a CI/CD platform integrated with GitLab. Pipelines are defined in YAML code in file named ```.gitlab-ci.yml```.

GitLab is quite similar to GitHub:
* Runners are the same - Shared runners are GitLab-hosted, self-hosted are still self-hosted
* Pipeline consists of stages \(which are another layer above jobs\) that consist of jobs \(analogous to GitHub Actions jobs\), script commands are analogous to GitHub Actions steps

Example pipeline YAML:
```
image: <host_image>
include: <path_to_file>

before_script:
  - <commands>
cache:
  key: <cache_key>
  paths:
    - <cache_path>

variables:  
  <variable_name>: <variable_value>

workflow:
  rules:
    - if: <condition>

stages:
  - <stage_name>

<job_name>:
  stage: <stage_name>
  extends: <job_name>
  trigger:
    project: <project_name>
    include: <child_pipeline_path>
  services:  
    - <service_name>  
  environment:
    name: <environment>
    url: <environment_url>
  script:
    - <command>
  artifacts:  
    paths:  
      - <artifact_path>  
  rules:
    - if: <condition>
```
Pipeline YAML file consists of:
* **image** - specifies Docker image that the job runs in
* **include** - include external YAML pipeline into this pipeline
* **before_script** - specify commands to run before each job
* **cache** - specify files to cache between jobs in specified path and with unique identifying key
* **variables** - defines variables
* **workflow** - controls pipeline behaviour
  * **rules** - prevents pipeline from running unless defined condition is met
* **stages** - stages of pipeline
  * **stage** - jobs stage
  * **extends** - reuse other jobs defined in this pipeline
  * **trigger** - triggers pipeline in other project or child pipeline in current project
  * **services** - specifies additional Docker images that are required
  * **environment** - defines the environment that a job deploys to, with name and url
  * **script** - run specified cli command
  * **artifacts** - specify files to save as job artifacts in specified path
  * **rules** - prevents job from running unless defined condition is met

To test pipelines locally use [gitlab-ci-local](https://github.com/firecow/gitlab-ci-local).
## Jenkins
Jenkins is open-source, self-hosted CI/CD platform. Pipelines are defined in Jenkinsfile that uses Groovy. It supports declarative or scripted syntax.

Jenkins is extendable by plugins which enhance its abilities. There's a lot of available plugins.
Examples of tools and systems that plugins integrate with:
* **Version control systems** - e.g. Git
* **Building tools** - e.g. Maven
* **Testing systems** - e.g. Selenium
* **Cloud platforms** - e.g. AWS
* **Containerization tools** - e.g. Docker, k8s

If you need plugin - just find and use it.

Jenkins architecture is based on Master and Agent nodes, where Master nodes are central servers that manage jobs, store configuration, etc. and Agent nodes execute given tasks. Agent nodes can be scaled according to workload.

Jenkins, similarly to earlier described tools, defines stages of pipeline and steps that they consist of.
Example declarative Jenkinsfile pipeline:
```
pipeline {
  agent <agent>

  environment {}

  options {}

  parameters {}

  triggers {}

  tools {}

  when {}

  matrix {
    axes {
      axis {
        name <axis_name>
        values <platform1>, <platform2>
      }
    }
  }

  stages {
    stage(<stage_name>) {
      steps {
        <command>
        script {}
      }
    }
    parallel {
      stage(<stage_name>) {
        steps {
          <command>
        }
      }
      stage(<stage_name>) {
        steps {
          <command>
        }
      }
    }
  }
}
```
Pipeline Jenkinsfile consists of:
* **agent** - specifies on which agent pipeline will be executed
* **stage** - jobs stage
* **environment** - defines environment variables, use credentials\(\) to access predefined Credentials by their identifier in the Jenkins environment
* **options** - allows for configuring pipeline-specific options from within the pipeline itself, check documentation for available options
* **parameters** - provides a list of parameters that a user should provide when triggering the pipeline, they have different types \(string, text, booleanParam, choice, password\) and consist of name, defaultValue or choices and description
* **triggers** - defines the automated ways in which the pipeline should be re-triggered, trigger types are cron, pollSCM or upstream, use Jenkins cron syntax to define time intervals
* **tools** - defines tools to auto-install and put on the PATH
* **when** - allows the pipeline to determine whether the stage should be executed depending on the given condition, check documentation for available conditions
* **parallel** - specifies a list of nested stages to be run in parallel
* **matrix** - defines a multi-dimensional matrix of name-value combinations to be run in parallel, similar to GitHub Actions
* **script** - specifies a block of scripted pipeline and executes that in the declarative pipeline
