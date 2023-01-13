# Azure Governed Pipelines

**In development**

This repository contains sample code you can use to create your own governed pipelines for deploying Azure solutions.
To learn more about PSRule for Azure, see <https://aka.ms/ps-rule-azure>.

[![Use this template](https://img.shields.io/static/v1?label=GitHub&message=Use%20this%20template&logo=github&color=007acc)](https://github.com/Azure/PSRule.Rules.Azure-governed/generate)

## Overview

Building enterprise scale pipelines can be challenging.
Without a standard approach, pipelines can become complex and difficult to maintain.
Governed pipelines provides a set of patterns to help you build and maintain pipelines for deploying Azure solutions.

- **Who should consider using governed pipelines?** &mdash; Any organzation that need to scale one or two pipelines to many.
- **Why use governed pipelines?** &mdash; A central set of governed pipelines allows you to managed quality and security across projects or teams.
- **What systems are supported?** &mdash; In the current interation governed pipelines support Azure Pipelines.
  In the future we plan to add support for GitHub Actions.

## How do they work?

Governed Pipelines allow an organization to enforce controls within deployment pipelines by extending on built-in features of Azure and GitHub.

Pipelines **must** prove they have extended from a secure pipeline that enforces an organization's DevSecOps requirements.

- When a pipeline extends from a governed pipeline it is entitled to use credentials with permissions to deploy to Azure.
- If the pipeline does not use a governed pipeline the credentials are not provides to the pipeline and deployment is blocked.

The implementation for Azure Piplines and GitHub Action is slightly different as follows:

- **Azure Pipelines** &mdash; [Security through templates][1] requires a specific pipeline template to be used.
- **GitHub Actions (available in the future)** &mdash; [Open ID Connect with reusable workflows][3] requires a specific workflow template to be used.

  [1]: https://learn.microsoft.com/azure/devops/pipelines/security/templates?view=azure-devops
  [3]: https://docs.github.com/enterprise-cloud@latest/actions/deployment/security-hardening-your-deployments/using-openid-connect-with-reusable-workflows

## Getting started

To get started, please check out our consumer guide for:

- [Azure Pipelines](docs/consumer-azure-pipelines.md)

## Support

This project uses GitHub Issues to track bugs and feature requests.
Please search the existing issues before filing new issues to avoid duplicates.

- For new issues, file your bug or feature request as a new [issue].
- For help, discussion, and support questions about using this project, join or start a [discussion].

Support for this project/ product is limited to the resources listed above.

  [issue]: https://github.com/Azure/PSRule.Rules.Azure-governed/issues
  [discussion]: https://github.com/Azure/PSRule.Rules.Azure-governed/discussion

## Contributing

This project welcomes contributions and suggestions.
If you are ready to contribute, please visit the [contribution guide](CONTRIBUTING.md).

## Code of Conduct

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/)
or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Maintainers

- [Bernie White](https://github.com/BernieWhite)
- [Sam Bell](https://github.com/ms-sambell)

## License

This project is [licensed under the MIT License](LICENSE).

## Trademarks

This project may contain trademarks or logos for projects, products, or services.
Authorized use of Microsoft trademarks or logos is subject to and must follow [Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.
