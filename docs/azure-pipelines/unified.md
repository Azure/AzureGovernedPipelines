# Unified pipeline

The unified pipeline is a pipeline template that can be used to create a governed pipeline.
Using this template, you can accelerate adoption of governed pipelines used by your organization to deploy to Azure.
The template is designed to be used with the [Security through templates][1] feature of Azure Pipelines.

The unified pipeline is not intended to be used directly.

While the template provides a default configuration, it can be customized to meet the needs of your organization.
For example, enforce third-party DevSecOps tools, or add custom stages to the pipeline.

  [1]: https://learn.microsoft.com/azure/devops/pipelines/security/templates?view=azure-devops

## Overview

The unified pipeline is structured to automatically inject in stages and steps that enforce DevSecOps controls.
The following features are included:

- **Prerequisites** &mdash; A stage that runs before all other stages to enforce mandatory controls.
  - This stage can be disabled or configured to not break the pipeline.
  - Custom or third-party tools can be added to this stage to enforce additional controls.
  - Several built-in tools can be enabled within this stage to enforce DevSecOps controls for Azure.
- **Built-in tools** &mdash; Built-in tools that can be used to enforce DevSecOps controls minimizes integration effort.
  These tools include:
  - Microsoft Defender for DevOps.
  - Microsoft PSRule for Azure.
- **Customizable stages** &mdash; The pipeline can be customized to add additional stages by consuming pipelines.
  This allows developers and DevOps engineers to develop pipelines to meet their needs.

## Usage

To use the unified pipeline, define your governed pipeline YAML by using it as a stage template.
The governed pipeline you define will be extended by each consuming pipeline.
This approach allows you to define mandatory controls that can not be overriden by consuming pipelines.

```yaml
parameters:
  # Settings are exposed to consuming pipelines to allow customization of options.
  - name: settings
    type: object
    default: {}

  # Stages are exposed to consuming pipelines to allow customization of stages.
  - name: stages
    type: stageList
    default: []

resources:
  repositories:
    - repository: AzureGovernedPipelines
      type: github
      name: Azure/AzureGovernedPipelines
      ref: refs/tags/v1.0.0

stages:
  - template: pipelines/unified.yml@AzureGovernedPipelines
    parameters:
      # Define mandatory options here.
      global: {}

      # Define option defaults here.
      defaults: {}

      # Allow consuming pipelines to override.
      settings: ${{ parameters.settings }}

      # Allow consuming pipelines to customize stages.
      stages: ${{ parameters.stages }}
```

Continue reading for more information on the avilaible parameters for customization.

## Parameters

The unified pipeline supports the following parameters for customization.

### `global`

Allows configuration of options within the governed pipeline that are enforced to all pipelines.
Options configured here will override options specified in `defaults` and `settings`.

To default a configuration option but allow it to be overridden, specify the option in `defaults` instead.

For a list of configuration options see [Options](#options).

### `defaults`

Allows configuration of option defaults for the governed pipeline.
Options configured here will be the default for all consuming pipelines.
Consuming pipelines can override the default by specifying the option in `settings`.

Any options specified here will always be overriden by options specified in `global`.

For a list of configuration options see [Options](#options).

### `settings`

Allows configuration of options within the governed pipeline by consuming pipelines.
Options configured here can be overriden by consuming pipelines.
For example, you may want to allow a pipeline turn on or off a feature.

When not specified, the effective configuration will be inherited from `defaults` unless the option is specified in `global`.

For a list of configuration options see [Options](#options).

When defining your governed pipeline, expose this parameter to allow consuming pipelines to override options.

```yaml
parameters:
  # Settings are exposed to consuming pipelines to allow customization of options.
  - name: settings
    type: object
    default: {}

  # Stages are exposed to consuming pipelines to allow customization of stages.
  - name: stages
    type: stageList
    default: []

resources:
  repositories:
    - repository: AzureGovernedPipelines
      type: github
      name: Azure/AzureGovernedPipelines
      ref: refs/tags/v1.0.0

extends:
  - template: pipelines/unified.yml@AzureGovernedPipelines
    parameters:
      # Define mandatory options here.
      global: {}

      # Define option defaults here.
      defaults: {}

      # Allow consuming pipelines to override.
      settings: ${{ parameters.settings }}

      # Allow consuming pipelines to customize stages.
      stages: ${{ parameters.stages }}
```

### `stages`

Allows consuming pipelines to add additional stages to the pipeline.
To configure pipeline stages, the consuming pipeline will specify a list of stages to add.

For example, to add a stage named `build`:

```yaml
extends:
  template: your-template.yml@GovernedPipelines
  parameters:
    stages:
      - stage: build_stage
        displayName: Build
        jobs:
          - job: build_job
            displayName: Build
            steps:
              - script: echo Hello, world!
                displayName: 'Run an example script'
```

## Options

The following options can be configured for customization of the governed pipeline.
These options can be set by configuring `global`, or `defaults`.
Consuming pipelines can override options specified in `defaults` by configuring the options in `settings`.

If the option is not set, the effective configuration will be inherited from the pipeline defaults described in this section.

```yaml
prereqs:
  enabled: boolean
  break: boolean

tools:
  psrule:
    enabled: boolean
    modules: string
    minimumVersion: string
```

For example, setting a default value for `tools.psrule.enabled`:

```yaml
extends:
  template: your-template.yml@GovernedPipelines
  parameters:
    # Customize the pipeline here by specifying parameters
    stages: []
    defaults:
      tools:
        psrule:
          enabled: true
```

### `prereqs.enabled`

Determines if the prerequisite (_Prereqs_) stage is enabled.
By default, this stage is enabled and will run before all other stages.

Syntax:

```yaml
prereqs:
  enabled: boolean
```

Default:

```yaml
prereqs:
  enabled: true
```

Examples:

```yaml
global:
  prereqs:
    enabled: false
```

### `prereqs.break`

Determines if a prerequisite (_Prereqs_) stage failure breaks the pipeline.
By default, a failure in the prereqs stage will halt the pipeline and fail.
Subsequent stages will not be run.

Syntax:

```yaml
prereqs:
  break: boolean
```

Default:

```yaml
prereqs:
  break: true
```

Examples:

```yaml
global:
  prereqs:
    break: false
```

### `tools.psrule.enabled`

Enables or disables scanning of the source repository using PSRule.

Syntax:

```yaml
tools:
  psrule:
    enabled: boolean
```

Default:

```yaml
tools:
  psrule:
    enabled: false
```

Examples:

```yaml
global:
  tools:
    psrule:
      enabled: true
```

### `tools.psrule.modules`

Configures the PSRule modules that will be used to during scanning.

Syntax:

```yaml
tools:
  psrule:
    modules: string
```

Default:

```yaml
tools:
  psrule:
    modules: false
```

Examples:

```yaml
global:
  tools:
    psrule:
      modules: 'PSRule.Rules.Azure'
```

### `tools.psrule.minimumVersion`

Configures the minimum version of PSRule that may be used.

Syntax:

```yaml
tools:
  psrule:
    minimumVersion: string
```

Default:

```yaml
tools:
  psrule:
    minimumVersion: 2.7.0
```

Examples:

```yaml
global:
  tools:
    psrule:
      minimumVersion: 2.7.0
```
