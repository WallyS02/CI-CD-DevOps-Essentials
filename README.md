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
### Building
### Testing
#### Unit Tests
#### Integration Tests
#### End-To-End \(E2E\) Tests
#### Load / Performance Tests
#### DAST
### Containerizing
### Deploying
#### Strategies
## GitHub Actions
## GitLab CI/CD
## Jenkins
