# Community and Support

<cite>
**Referenced Files in This Document**   
- [CONTRIBUTING.md](file://CONTRIBUTING.md)
- [CODE_OF_CONDUCT.md](file://CODE_OF_CONDUCT.md)
- [README.md](file://README.md)
- [CHANGELOG.md](file://CHANGELOG.md)
- [LICENSE.md](file://LICENSE.md)
</cite>

## Table of Contents
1. [Introduction](#introduction)
2. [Contribution Process](#contribution-process)
3. [Code of Conduct](#code-of-conduct)
4. [Getting Help](#getting-help)
5. [Bug Reporting](#bug-reporting)
6. [Feature Requests](#feature-requests)
7. [Enterprise Support](#enterprise-support)
8. [Documentation Contributions](#documentation-contributions)
9. [Security Disclosure](#security-disclosure)
10. [Project Governance](#project-governance)

## Introduction
The Unstructured project is an open-source ecosystem for preprocessing pipeline APIs and supporting libraries designed to streamline and optimize data processing workflows for large language models (LLMs). This document provides comprehensive information about community resources, support channels, contribution processes, and governance models for the Unstructured project. The project welcomes contributions from the community and provides various channels for getting help, reporting issues, and requesting features.

**Section sources**
- [README.md](file://README.md#L1-L256)

## Contribution Process
Contributing to the Unstructured project involves following a structured process to ensure code quality and maintain project standards. Contributors should start by exploring the GitHub issues tab to find interesting issues, particularly those labeled as "good first issue" or "contributions welcome" which are well-suited for outside contributions.

Before creating a pull request, contributors must complete the following checklist:
- Ensure all new classes, functions, and methods have docstrings
- Add type hints to new functions and methods (optional for tests)
- Write associated tests for new functionality
- Update CHANGELOG.md and __version__.py if core code has changed
- Update dependencies in .in files if needed and recompile with `make pip-compile`
- Run tests locally with `make test` and ensure all tests pass
- Run typing, linting, and formatting checks with `make check`
- Remove debugging artifacts and commented-out code
- Use the standard format `# NOTE(<username>): <comment>` for comments

Pull requests should follow conventional commit standards with titles formatted as `<type>: <description>`. Common commit types include `feat` (new feature), `fix` (bug fix), `chore` (non-functional changes), `refactor` (code restructuring), and `docs` (documentation updates). Feature branches should follow the naming convention `<username>/<description>`.

**Section sources**
- [CONTRIBUTING.md](file://CONTRIBUTING.md#L1-L135)

## Code of Conduct
The Unstructured project adheres to the Contributor Covenant Code of Conduct to foster an open and welcoming environment for all participants. The community pledges to make participation harassment-free for everyone regardless of age, body size, disability, ethnicity, gender identity and expression, level of experience, nationality, personal appearance, race, religion, or sexual identity and orientation.

Community members are expected to demonstrate empathy and kindness, respect differing opinions, give and accept constructive feedback, take responsibility for mistakes, and focus on what is best for the overall community. Unacceptable behavior includes sexualized language or imagery, trolling, insulting comments, personal or political attacks, harassment, publishing others' private information without permission, and other conduct inappropriate in a professional setting.

The Code of Conduct applies to all community spaces and when individuals are officially representing the community in public spaces. Community leaders are responsible for enforcing these standards and have the right to remove, edit, or reject comments, commits, code, wiki edits, issues, and other contributions that violate the Code of Conduct.

**Section sources**
- [CODE_OF_CONDUCT.md](file://CODE_OF_CONDUCT.md#L1-L129)
- [CONTRIBUTING.md](file://CONTRIBUTING.md#L98-L108)

## Getting Help
Users can access help through multiple channels. The primary community platform is the Slack workspace, which can be joined via the invitation link provided in the README. The project also maintains a presence on LinkedIn for professional networking and updates.

For technical assistance, users can:
- Consult the comprehensive documentation at https://docs.unstructured.io
- Review the GitHub repository issues for existing solutions
- Participate in discussions on the project's Slack channel
- Explore the example documentation and test files in the repository
- Refer to the detailed installation and usage instructions in the README

The project provides extensive documentation covering core functionality, connectors, concepts, and integrations. Users are encouraged to review the Quick Start guide, documentation on using the unstructured open-source package, connector information, and integration guides to get started with the library.

**Section sources**
- [README.md](file://README.md#L1-L256)

## Bug Reporting
When encountering a bug, users should create a new GitHub issue using the bug report template. To help the development team diagnose the issue effectively, users should include environment information gathered by running the command `python scripts/collect_env.py`. This command collects system information that helps identify potential compatibility or configuration issues.

Bug reports should include:
- A clear description of the problem
- Steps to reproduce the issue
- Expected behavior versus actual behavior
- Environment information from `collect_env.py`
- Any relevant error messages or logs
- Sample code or documents that trigger the issue (when possible)

The development team reviews all bug reports and prioritizes fixes based on severity and impact. Security-related bugs should be reported through the responsible disclosure process rather than public issue tracking.

**Section sources**
- [README.md](file://README.md#L241-L243)

## Feature Requests
Feature requests can be submitted through the GitHub issue tracker. Users should provide detailed information about the desired functionality, including:
- A clear description of the proposed feature
- Use cases and scenarios where the feature would be beneficial
- Potential implementation approaches (if known)
- Any relevant examples or references

The project maintainers review feature requests and prioritize them based on community interest, alignment with project goals, and resource availability. Users are encouraged to participate in discussions on feature requests to provide additional context and use cases.

For complex feature requests, contributors may be asked to provide a proof of concept or prototype implementation. The project welcomes contributions that implement new features, provided they follow the contribution guidelines and pass all quality checks.

**Section sources**
- [README.md](file://README.md#L241-L243)

## Enterprise Support
Enterprise users have access to the Unstructured Platform product, which offers advanced features beyond the open-source library. The enterprise solution provides better processing performance and additional capabilities including chunking, embedding, and image and table enrichment generation through a low-code UI or API.

Enterprise customers can request a demo from the sales team to learn more about production deployment options and advanced features. The platform is designed for organizations that need to move their data processing pipelines to production with enhanced capabilities and support.

While the open-source project provides community support through GitHub and Slack, enterprise customers receive dedicated support services, SLAs, and priority access to new features and updates.

**Section sources**
- [README.md](file://README.md#L42-L44)

## Documentation Contributions
The Unstructured project welcomes contributions to its documentation. Documentation improvements can be submitted as pull requests following the same contribution process as code changes. The project uses the `docs` commit type for documentation-related changes.

Documentation contributions should:
- Follow the project's style guidelines
- Provide clear and concise explanations
- Include relevant examples where appropriate
- Maintain consistency with existing documentation
- Update related sections when making changes

The project documentation is hosted at https://docs.unstructured.io and covers various aspects including installation, core functionality, partitioning, connectors, and integrations. Contributors are encouraged to improve existing documentation and add new guides based on their experiences with the library.

**Section sources**
- [CONTRIBUTING.md](file://CONTRIBUTING.md#L87-L88)

## Security Disclosure
The project has a dedicated security policy for reporting vulnerabilities. Security issues should not be reported through the public GitHub issue tracker to prevent potential exploitation before fixes are developed and deployed.

Users who discover security vulnerabilities should follow the security policy available at https://github.com/Unstructured-IO/unstructured/security/policy. This ensures that vulnerabilities are handled responsibly and fixes can be developed and released before public disclosure.

The security policy outlines the process for reporting vulnerabilities, expected response times, and coordination for public disclosure. This responsible disclosure process protects users by allowing the development team to address security issues before they become widely known.

**Section sources**
- [README.md](file://README.md#L236-L239)

## Project Governance
The Unstructured project is governed by a team of maintainers who are responsible for reviewing contributions, enforcing the Code of Conduct, and guiding the project's technical direction. Community leaders have the responsibility to clarify and enforce standards of acceptable behavior and take appropriate corrective action when necessary.

The enforcement guidelines outline a progressive response system for Code of Conduct violations:
1. **Correction**: A private, written warning explaining the nature of the violation, with a possible request for a public apology
2. **Warning**: A formal warning with consequences for continued behavior, including temporary interaction restrictions
3. **Temporary Ban**: A temporary ban from community interactions for serious violations
4. **Permanent Ban**: A permanent ban for patterns of violation or severe misconduct

Maintainers who fail to follow or enforce the Code of Conduct in good faith may face temporary or permanent repercussions as determined by other members of the project's leadership. The project operates under the Apache 2.0 license, which governs contributions and usage rights.

**Section sources**
- [CODE_OF_CONDUCT.md](file://CODE_OF_CONDUCT.md#L69-L114)
- [CONTRIBUTING.md](file://CONTRIBUTING.md#L54-L61)
- [LICENSE.md](file://LICENSE.md#L1-L202)