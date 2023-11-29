# Unified pipeline

The unified pipeline is a pipeline template that can be used to create a governed pipeline.
Using this template, you can accelerate adoption of governed pipelines used by your organization to deploy to Azure.
The template is designed to be used with the [Security through templates][1] feature of Azure Pipelines.

While the template provides a default configuration, it can be customized to meet the needs of your organization.
For example, enforce third-party DevSecOps tools, or add custom stages to the pipeline.

  [1]: https://learn.microsoft.com/azure/devops/pipelines/security/templates?view=azure-devops

## Usage

To use the unified pipeline, add the following to your pipeline YAML:

```yaml
extends:
  template: your-template.yml@GovernedPipelines
  parameters:
    # Customize the pipeline here by specifying parameters
    stages: []
```

Continue reading for more information on the avilaible parameters for customization.

## Parameters

### `global`

The `global` parameter allows configuration of the governed pipeline through several options.
Additional user defined options can be added to `global` for pipeline customization.
For a list of configuration options see [Options](#options).

### `defaults`

The `defaults` parameter allows configuration of the global pipeline and provides the same options as `global`.
This parameter differs from `global` because it allow you to configure the default behaviour when an option in `global` is not set.

### `settings`

The `settings` parameter allows configuration of the governed pipeline options by consuming pipelines.
For example, you may want to allow a pipeline turn on or off a feature.

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

The following options can be configured for pipeline customization.
These options can be set by configuring `global` or `defaults`.
When neither of these are set, the effective configuration will be inherited from the pipeline defaults described in this section.

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

### `prereqs.enabled`

Determines if the prerequsist stage is enabled.
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

Determines if a prereq stage failure breaks the pipeline.
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
