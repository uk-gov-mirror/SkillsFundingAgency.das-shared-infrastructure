# Apprenticeship Service Shared Infrastructure

|  | Status |
|-------------|--------|
| Build (Master) | [![Build Status](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_apis/build/status%2Fdas-shared-infrastructure?repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master)](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_build/latest?definitionId=4309&repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master)
| DTA | [![Build Status](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_apis/build/status%2Fdas-shared-infrastructure?repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master&stageName=Deploy%20to%20DTA)](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_build/latest?definitionId=4309&repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master) |
| Dev/Test | [![Build Status](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_apis/build/status%2Fdas-shared-infrastructure?repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master&stageName=Deploy%20to%20DEV)](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_build/latest?definitionId=4309&repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master) |
| AT | [![Build Status](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_apis/build/status%2Fdas-shared-infrastructure?repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master&stageName=Deploy%20to%20AT)](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_build/latest?definitionId=4309&repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master) |
| TEST | [![Build Status](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_apis/build/status%2Fdas-shared-infrastructure?repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master&stageName=Deploy%20to%20TEST)](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_build/latest?definitionId=4309&repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master) |
| TEST2 | [![Build Status](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_apis/build/status%2Fdas-shared-infrastructure?repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master&stageName=Deploy%20to%20TEST2)](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_build/latest?definitionId=4309&repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master) |
| DEMO | [![Build Status](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_apis/build/status%2Fdas-shared-infrastructure?repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master&stageName=Deploy%20to%20DEMO)](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_build/latest?definitionId=4309&repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master) |
| PP | [![Build Status](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_apis/build/status%2Fdas-shared-infrastructure?repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master&stageName=Deploy%20to%20PREPROD)](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_build/latest?definitionId=4309&repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master) |
| MO | [![Build Status](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_apis/build/status%2Fdas-shared-infrastructure?repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master&stageName=Deploy%20to%20MO)](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_build/latest?definitionId=4309&repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master) |
| PROD | [![Build Status](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_apis/build/status%2Fdas-shared-infrastructure?repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master&stageName=Deploy%20to%20PROD)](https://dev.azure.com/sfa-gov-uk/Apprenticeships%20Service%20Cloud%20Platform/_build/latest?definitionId=4309&repoName=SkillsFundingAgency%2Fdas-shared-infrastructure&branchName=master) |

The templates hosted in this repository facilitate the deployment of the shared infrastructure for the Apprenticeships Service.

The deployment consists of two layers.

### Subscription layer
The subscription layer owns resources that are shared horizontally across a subscription and are used for management purposes. For example, Log Analytics, Azure Automation, KeyVault, Storage, Alerting, Dashboards etc.

**Note**: A subscription can contain one or more environments.

### Environment layer
The environment layer owns resources that are shared vertically across an environment and are typically used to provide a platform for other independent applications. For example; App Service Plans, Virtual Networks, SQL Servers, ServiceBus etc.

## Shared Log Analytics workspaces

Each environment has two shared Log Analytics workspaces, deployed into `das-{env}-shared-rg` by [environment.template.json](templates/environment.template.json) from the reusable [log-analytics-workspace.json](https://github.com/SkillsFundingAgency/das-platform-building-blocks/blob/master/templates/log-analytics-workspace.json) building block.

| Workspace | Purpose | Analytics retention | Total retention |
| --- | --- | --- | --- |
| `das-{env}-shared-ops-la` | Availability, performance and troubleshooting telemetry | `operationalLogAnalyticsRetentionInDays` (default 90) | same |
| `das-{env}-shared-sec-la` | Audit and security relevant resource logs | `securityLogAnalyticsRetentionInDays` (default 90) | `securityLogAnalyticsTotalRetentionInDays` (default 365) |

Data between the analytics retention and the total retention is held in low cost long term retention and retrieved with a [search job](https://learn.microsoft.com/azure/azure-monitor/logs/search-jobs). Both values can be changed later without data loss - increasing total retention applies to data already ingested, and shortening it has a 30 day grace period before removal.

These are new workspaces. The pre-existing per subscription workspace (`logAnalyticsWorkspaceName`, in `das-{sub}-mgmt-rg`) is unchanged and still receives Application Insights, Service Bus and Event Hub diagnostics.

### Long term retention and the table list

Long term retention is applied per table via `securityLogAnalyticsTableNames`. A table can only be configured once it exists in the workspace, so this list is empty until the resources that populate it are onboarded. **Until then the security workspace holds 90 days and no long term retention** - the one year requirement is not met by deploying this repository alone.

### Immutable archive

`Microsoft.OperationalInsights/workspaces` has no immutability property. Tamper protection follows [Microsoft's documented pattern](https://learn.microsoft.com/azure/azure-monitor/logs/logs-data-export): a [data export rule](https://github.com/SkillsFundingAgency/das-platform-building-blocks/blob/master/templates/log-analytics-workspace-data-export.json) writes the security tables to a storage account with a WORM immutability policy, deployed by [storage-account-immutable.json](https://github.com/SkillsFundingAgency/das-platform-building-blocks/blob/master/templates/storage-account-immutable.json) as `das{env}sharedsecstr`.

`securityLogAnalyticsImmutabilityPolicyState` controls the policy:

| State | Behaviour |
| --- | --- |
| `Disabled` | No immutability. **Required for DTA**, which is torn down and rebuilt by [Remove-Environment.ps1](tests/Remove-Environment.ps1). |
| `Unlocked` | Protects against overwrite. Can still be removed and the account deleted. |
| `Locked` | **Irreversible.** The blobs, container, storage account and resource group cannot be deleted until the immutability period expires. Cannot be overridden by subscription administrators or Microsoft Support. |

A policy can only be created `Disabled` or `Unlocked`; only an `Unlocked` policy can transition to `Locked`. Roll out `Unlocked` first, confirm the export is landing blobs, then move approved environments to `Locked` with the approval recorded.

### Splunk

Security logs reach Splunk through a second data export rule targeting an Event Hubs namespace, consumed by the Splunk Add-on for Microsoft Cloud Services. It is off by default (`deploySecurityLogSplunkExport`) until the Splunk side is listening, because exporting to a destination that is not consuming the data costs money for no benefit.

Data export cannot filter rows - it exports whole tables from the point the rule is created - so targeting is expressed as a short list of tables in `securityLogSplunkTableNames`. `securityLogSplunkEventHubNamespaceResourceId` is supplied by configuration so an existing namespace can be reused. Both Splunk string parameters use `none` rather than an empty string as their unset value, because the parameters file builder treats empty values as missing.

## External dependencies
There is a third layer that is not deployed by these templates. This is the application layer. Deployment templates for applications within this layer are typically stored with the [application code](https://github.com/SkillsFundingAgency/das-reservations/tree/master/azure) as they will share the same lifecycle. These applications will often depend on infrastructure deployed by templates in this repository.

Both shared and application deployments consume templates from the [platform building blocks](https://github.com/SkillsFundingAgency/das-platform-building-blocks) repository.

## Logical view
The diagram below is a logical representation of the deployment template structure.

![ApprenticeshipsSharedInfrastructure](images/ApprenticeshipsSharedInfrastructure-v1.2.png)

## Deployment

### Azure DevOps deployments
This is the primary method used to deploy the infrastructure. Configuration is stored securely either in the build definition or variable groups and versioned artifacts are used when deploying.

### Local deployment
 To deploy from your local machine, set each parameter as an environment variable then run the script below.

``` PowerShell
.\Initialize-SharedInfrastructureDeployment.ps1
```

The expected environment variable names can be found in the **metadata** object for each parameter in [subscription.template.json](templates/subscription.json).

For example:

``` Javascript
"resourceEnvironmentName": {
    "type": "string",
    "metadata": {
        "description": "Base environment name. E.g. DEV. PP, PRD, MO. ",
        "environmentVariable": "resourceEnvironmentName"
    }
}
```

**Note**: This deployment method should ***only*** be used for testing purposes.

## Testing

Use the unit test runner to execute tests. The LocalDeployment switch will execute tests in a new process to avoid polluting the current runspace.

```PowerShell
.\Start-UnitTest.ps1 -LocalDeployment
```

You can specify specific tests by using the Path parameters.

```PowerShell
.\Start-UnitTest.ps1 -Path .\Initialize-SharedInfrastructureDeployment.tests.ps1 -LocalDeployment
```

The example above will execute the end to end mock deployment test.

New tests are automatically invoked at build time and the run will produce a test report which is published back to azure-pipelines in the context of the build.

## DTA Deployment

The DTA stage deploys and tears down the infrastructure to provide a method for testing the deployment process without interferring with the availablity of the AT environment.

### Building block dependency

The three templates above live in [das-platform-building-blocks](https://github.com/SkillsFundingAgency/das-platform-building-blocks) and are resolved from that repository's `master` branch at deployment time, not from the pinned tag in [azure-pipelines.yml](azure-pipelines.yml). The tag governs the pipeline YAML step templates only. A building blocks change is therefore live for every consumer on merge, which is why those templates default every new parameter to the existing behaviour.
