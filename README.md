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
## GitLab CI/CD
## Jenkins
