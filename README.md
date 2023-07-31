#  Itential Automation Start Action

A Github action that will start Itential automations.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Supported Itential Automation Platform Versions](#supported-itential-automation-platform-versions)
- [Getting Started](#getting-started)
- [Configurations](#configurations)
  - [Required Input Parameters](#required-input-parameters)
  - [Required Authentication Parameters](#required-authentication-parameters)
  - [Optional Input Parameters](#optional-input-parameters)
  - [Output](#output)
- [Example Usage](#example-usage)

## Prerequisites

An Itential account is required to get credentials needed to configure the Github Actions.
In order to utilize this action, you would need to have an active `Itential Automation Platform` (IAP) / `Itential Cloud` instance.\
If you are an existing customer, please contact your Itential account team for additional details.
For new customers interested in an Itential trial, please click [here](https://www.itential.com/get-started/) to request one.

## Supported Itential Automation Platform Versions

- 2023.1
- 2022.1
- 2021.2
- 2021.1

## Getting Started

1. Search for the Action on Github Marketplace.
2. Select the "Use the Latest Version" option on the top right of the screen.
3. Click the clipboard icon to copy the provided data.
4. Navigate to the '.github/workflows' file in the target repository (where you intend on using the action).
5. Paste the copied data in the correlating fields.
6. Configure the required inputs and optional inputs. (See note)
7. Save it as a main.yml file.

> **_Note:_** Users may manually enter required input parameters or use Github Secrets if they want to hide certain parameters. If you choose to use Github Secrets, please reference the instructions provided below.

1. Select the settings tab on your target repository.
2. Select the secrets and variables tab under security options.
3. Click the "new repository secret"option on the top right of the screen.
4. Enter the required fields.
  For YOUR_SECRET_NAME enter a required input.
  For SECRET enter your desired variable.
5. Click "Add Secret"

_For more information about Github Actions variables, see [variables](https://docs.github.com/en/actions/learn-github-actions/variables)_

## Configurations

See [action.yml](action.yml) for [metadata](https://docs.github.com/en/actions/creating-actions/metadata-syntax-for-github-actions) that defines the inputs, outputs, and runs configurations for this action.\_

_For more information about connecting to private network, see [Connecting to a private network](https://docs.github.com/en/actions/using-github-hosted-runners/connecting-to-a-private-network)_

### Required Input Parameters

The following table defines the required input parameters to run an Itential automation using a Github workflow.Input data is provided through Github Actions secrets. For more information about github action secrets, see [Github Secrets](https://docs.github.com/en/rest/actions/secrets?apiVersion=2022-11-28)

| Parameter         | Description                                      |
| ----------------- | ------------------------------------------------ |
| itential_host_url | URL to the Itential Instance.                    |
| api_endpoint      | API endpoint name to start an automation.        |
| api_endpoint_body | The POST body used to create the workflow input. |

### Required Authentication Parameters

There are three methods for authenticating API requests to the Itential instance.The following tables describe the required parameters for each authentication method.Based on the available credentials input the appropriate parameters. By default, all the authentication parameters are set to empty string.

#### Static Token
The provided Static token will be used in all subsequent API calls to the Itential instance 

| Parameter         | Description                                      |
| ----------------- | ------------------------------------------------ |
| auth_token        |  Itential Authentication Token                   |

#### Basic Token 
The provided username and password is used to request a token that will be used in all subsequent API calls to the Itential instance . 

| Parameter         | Description                                      |
| ----------------- | ------------------------------------------------ |
| auth_username     | Itential Authentication Username                 |
| auth_password     | Itential Authentication Password                 |

#### Oauth2 Client Credentials
The provided client credentials are used to request a token that will be used in all subsequent API calls to the Itential instance .

| Parameter         | Description                                       |
| ----------------- | --------------------------------------------------|
| auth_client_id    | Itential Authentication Client ID                 |
| auth_client_secret| Itential Authentication Client Secret             |


### Optional Input Parameters

The following table defines three parameters considered optional.

**Note:** By default, automation_status is set to 1 and it will return the automation output. If you do not want the output returned and just want to start the automation, set automation_status to 0.

| Parameter         | Description                                               | Default Value |
| ----------------- | --------------------------------------------------------- | ------------- |
| automation_status | If user wants to check the status of the automation       | 1             |
| time_interval     | Time interval to check the automation status (in seconds) | 15            |
| no_of_attempts    | No. of attempts to check the automation status            | 10            |

### Output

| Parameter | Description                       |
| --------- | --------------------------------- |
| results   | Automation Start output variables |

## Example Usage

The example below displays how to configure a workflow that runs when issues or pull requests are opened or labeled in your repository. This workflow runs when new pull request is opened as defined in the `on` variable of the workflow.

This action is defined in `job1` of the workflow by the step id `step1`. The output of this step is defined as `output1` of the workflow and is extracted using the output variable of this action i.e. `results` as defined in the output.

You have the option to configure any filters you may want to add, such as only triggering workflow with a specific branch.

_For more information about workflows, see [Using workflows](https://docs.github.com/en/actions/using-workflows)._

`Github Workflow example `

```yaml
# This is a basic workflow to help you get started with Actions
name: Start automation on pull request
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
    name: Automation start
    steps:
      # To use this repository's private action, you must checkout the repository
      - name: Checkout
        uses: actions/checkout@v3
      - name: Itential Automation Start action step
        id: step1
        uses: itential/itential-automation-start@version_number
        with:
          #github_token: ${{secrets.GITHUB_TOKEN}} # include only if user requires a GitHub Token
          itential_host_url: ${{secrets.ITENTIAL_HOST_URL}}
          auth_token: ${{secrets.AUTH_TOKEN}}
          auth_username: ${{secrets.AUTH_USERNAME}}
          auth_password: ${{secrets.AUTH_PASSWORD}}
          auth_client_id: ${{secrets.AUTH_CLIENT_ID}}
          auth_client_secret: ${{secrets.AUTH_CLIENT_SECRET}}
          time_interval: 1
          no_of_attempts: 200
          api_endpoint: ${{secrets.API_ENDPOINT}}
          api_endpoint_body: ${{secrets.API_ENDPOINT_BODY}}
          automation_status: 1
      - name: Get output
        run: echo "${{steps.step1.outputs.results}}"
```
