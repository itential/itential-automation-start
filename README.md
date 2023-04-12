## Table of Contents 
  - [Overview](#overview)
  - [Prerequisites](#prerequisites)
  - [Supported IAP Versions](#supported-iap-versions)
  - [Getting Started](#getting-started)
  - [Configurations](#configurations)
    - [Required Input Parameters](#required-input-parameters)
    - [Optional Input Parameters](#optional-input-parameters)
    - [Output](#output)
  - [Example Usage](#example-usage)

## Overview
A Github action that will trigger Itential automations.

## Prerequisites  
An Itential account is required to get credentials needed to configure the Github Actions.
In order to utilize this action, you would need to have an active `Itential Automation Platform` (IAP).\
If you are an existing customer, please contact your Itential account team for additional details.
For new customers interested in an Itential trial, please click [here](https://www.itential.com/get-started/) to request one.

## Supported Itential Automation Platform(IAP) Versions

* 2022.1
* 2021.2
* 2021.1

## Getting Started
1. Search for the Action on Github Marketplace.
2. Select the "Use the Latest Version" option on the top right of the screen 
3. Click the clipboard icon to copy the provided data. 
4. Navigate to the 'github/workflows' file in the target repository (where you intend on using the action. )
5. Paste the copied data in the correlating fields. 
6. Configure the required inputs and optional inputs. (See note)
7.  Save it as a action.yml file.

Note: Users may manually enter required input parameters or use Github Secrets if they want to hide certain parameters. If you choose to use Github Secrets, please reference the instructions provided below. 

1. Select the settings tab on your target repository 
2. Select the secrets and variables tab under security options 
3. Click the "new repository secret"option on the top right of the screen 
4. Enter the required fields 
For YOUR_SECRET_NAME enter a required input. 
For SECRET enter your desired variable. 
6. Click "Add Secret"

_For more information about Github Actions variables, see [variables](https://docs.github.com/en/actions/learn-github-actions/variables)_


## Configurations
[action.yml](action.yml) file defines the metadata for this action.

_For more information about metadata, see [metadata](https://docs.github.com/en/actions/creating-actions/metadata-syntax-for-github-actions)._

### Required Input Parameters
The following table defines the four required parameters to run an IAP automation using a Github workflow. 

| Parameter | Description |
| --- | --- |
| IAP_INSTANCE | URL to the IAP Instance |
| IAP_TOKEN | To authenticate api requests to the instance |
| API_ENDPOINT | API endpoint name to trigger an automation |
| API\_ENDPOINT\_BODY | The POST body used to create the workflow input |

### Optional Input Parameters
The following table defines three parameters considered optional. 

**Note:** By default, JOB_STATUS is set to 1 and it will return the job output . If you do not want  the output returned and just want to trigger the automation, set JOB_STATUS to 0.

| Parameter | Description | Default Value |
| --- | --- | --- |
| JOB_STATUS | If user want to check the status of the job. | 1   |
| TIME_INTERVAL | Time interval to check job status | 15 seconds |
| NO\_OF\_ATTEMPTS | No of attempts to check job status | 10  |

### Output

| Parameter | Description |
| --------- | ----------- |
| results | API Trigger output variables |


## Example Usage

The example below displays how to configure a workflow that runs when issues or pull requests are opened or labeled in your repository. This workflow runs when new pull request is opened as defined in the `on` variable of the workflow.

This action is defined in  `job1` of the workflow by the step id `step1`. The output of this step is defined as `output1` of the workflow and is extracted using the output variable of this action i.e. `results` as defined in the output.

You have the option to configure any filters you may want to add, such as only triggering workflow with a specific branch. 

_For more information about workflows, see [Using workflows](https://docs.github.com/en/actions/using-workflows)._

`Github Workflow example `
```yaml
# This is a basic workflow to help you get started with Actions
name: Trigger automation on pull request
# Controls when the workflow will run
on:
  pull_request:
    types: [opened]

jobs:
  job1:
    runs-on: ubuntu-latest
    outputs:
      #step1 output
      output1: ${{ steps.step1.outputs.results }}
    name: API Endpoint Trigger
    steps:
      # To use this repository's private action, you must checkout the repository
      - name: Checkout
        uses: actions/checkout@v3
      - name: IAP Automation trigger action step
        id: step1
        uses: itential/itential-trigger-automation@version_number
        env:
          #Inputs required to run the action and trigger IAP automation
          IAP_INSTANCE: ${{secrets.IAP_INSTANCE}}
          IAP_TOKEN: ${{secrets.IAP_TOKEN}}
          API_ENDPOINT: ${{secrets.API_ENDPOINT}}
          API_ENDPOINT_BODY: ${{secrets.API_ENDPOINT_BODY}}
          #Additional inputs to wait for job completion and get output results.
          JOB_STATUS: 1
          NO_OF_ATTEMPTS: 10
          TIME_INTERVAL: 15
      - name: Get output
        run: echo "${{steps.step1.outputs.results}}"
```
