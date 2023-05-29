# nodejs-playwright

This repository contains a workflow for running Playwright tests using Node.js. The workflow is triggered by a workflow call and allows specifying the type of test to run.

## Inputs

- `test_type` (required): Type of test to run. Valid values are "sanity", "regression", "api", and "newFeature". The default value is "sanity".

## Workflow

The workflow consists of a single job named "run-test-and-upload-report" that runs the Playwright tests and uploads the test report as an artifact.

### Job: run-test-and-upload-report

- `timeout-minutes`: 60
- `runs-on`: ubuntu-latest
- `container`: mcr.microsoft.com/playwright:v1.34.0-focal

#### Steps

1. Checkout the repository using `actions/checkout@v3`.
2. Set up Node.js using `actions/setup-node@v3` with Node.js version 16.
3. Install project dependencies using `npm ci`.
4. Run Playwright tests by executing `npm run test:${{ inputs.test_type }}`.
5. Upload the test report as an artifact using `actions/upload-artifact@v3`.
   - `name`: playwright-report-${{ inputs.test_type }}
   - `path`: playwright-report/
   - `retention-days`: 30

Note: The artifact is always uploaded regardless of the test outcome (`if: always()`).


# Java Maven Selenium

This is a basic workflow that helps you get started with GitHub Actions. It runs tests for a Java project using Maven and TestNG, and uploads a build artifact.

## Workflow Triggers

The workflow is triggered in the following scenarios:

- Push events on the "main" branch.
- Pull request events targeting the "main" branch.
- Manual execution from the Actions tab (workflow_dispatch).

## Jobs

The workflow consists of a single job named "run-tests" that performs the following tasks:

1. Runs on: Ubuntu latest.
2. Container: Maven image.
3. Uses a Selenium standalone Chrome container as a service.

### Steps

1. Checks out your repository under `$GITHUB_WORKSPACE` using `actions/checkout@v3`.
   - Repository: 777abhi/java-maven-testng-actions
2. Runs Selenium tests by executing the following commands:
   - `ls`
   - `mvn clean`
   - `mvn -ntp -B -q clean test -Dsurefire.suiteXmlFiles=test_control/e2e_integration/newFeature.xml -Dwebdriver.chrome.url=http://selenium:4444/wd/hub`
3. Uploads a build artifact using `actions/upload-artifact@v3`. The artifact is always uploaded regardless of the test outcome (`if: always()`).
   - Name: target-folder
   - Path: target