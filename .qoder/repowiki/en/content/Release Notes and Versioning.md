# Release Notes and Versioning

<cite>
**Referenced Files in This Document**   
- [CHANGELOG.md](file://CHANGELOG.md)
- [setup.py](file://setup.py)
- [unstructured/__version__.py](file://unstructured/__version__.py)
- [scripts/check-new-release-version.sh](file://scripts/check-new-release-version.sh)
- [scripts/version-sync.sh](file://scripts/version-sync.sh)
- [Makefile](file://Makefile)
- [CONTRIBUTING.md](file://CONTRIBUTING.md)
</cite>

## Table of Contents
1. [Introduction](#introduction)
2. [Versioning Scheme](#versioning-scheme)
3. [Release Cycle](#release-cycle)
4. [Version History](#version-history)
5. [Breaking Changes and Migration Paths](#breaking-changes-and-migration-paths)
6. [Upgrade Guides](#upgrade-guides)
7. [Backward Compatibility and Deprecation Policies](#backward-compatibility-and-deprecation-policies)
8. [Long-Term Support and Security Updates](#long-term-support-and-security-updates)
9. [Version Increment Criteria](#version-increment-criteria)
10. [Feature Inclusion Process](#feature-inclusion-process)

## Introduction
This document provides comprehensive information about the versioning and release process for the unstructured library. It details the versioning scheme, release cycle, version history, breaking changes, migration paths, upgrade guides, backward compatibility guarantees, deprecation policies, long-term support, security updates, criteria for version increments, and feature inclusion processes. The unstructured library is an open-source toolkit for pre-processing unstructured data such as PDFs, HTML, Word documents, and other file types, preparing them for downstream machine learning tasks.

**Section sources**
- [README.md](file://README.md#L1-L256)

## Versioning Scheme
The unstructured library follows Semantic Versioning (SemVer) 2.0.0, using a three-part version number format: MAJOR.MINOR.PATCH. The current version is 0.18.20, as indicated in the `__version__.py` file. The versioning scheme is implemented through a combination of automated scripts and manual processes. The `scripts/version-sync.sh` script is responsible for synchronizing version numbers across different files in the repository, ensuring consistency between the CHANGELOG.md and the package version. The version number is stored in `unstructured/__version__.py` and is referenced in `setup.py` during package installation. The repository uses a pre-release versioning convention with the "-dev" suffix for development versions, as seen in the version check script that specifically identifies non-dev versions for release. The versioning process is integrated into the build system through the Makefile, which includes targets for version synchronization and checking.

**Section sources**
- [unstructured/__version__.py](file://unstructured/__version__.py#L1-L2)
- [setup.py](file://setup.py#L24-L25)
- [scripts/version-sync.sh](file://scripts/version-sync.sh#L1-L172)
- [Makefile](file://Makefile#L244-L270)

## Release Cycle
The unstructured library follows a continuous release cycle with frequent patch and minor releases. The release process is automated through a combination of scripts and CI/CD workflows. The `scripts/check-new-release-version.sh` script checks if the current version is a non-development version and differs from the main branch version, indicating a new release is ready. The release cycle includes several automated checks before deployment, including dependency consistency checks (`scripts/consistent-deps.sh`), license compliance checks, and version synchronization validation. The CONTRIBUTING.md document outlines the release checklist that contributors must follow, including updating the CHANGELOG.md and version file when core code changes, ensuring dependencies are properly compiled, and running all tests and checks locally. The Makefile contains targets for version synchronization and checking, which are part of the release validation process. Releases are published to PyPI, and Docker images are built and pushed to the Docker registry. The release cycle accommodates both regular feature releases and urgent security patches, as evidenced by multiple security fixes in recent versions.

**Section sources**
- [scripts/check-new-release-version.sh](file://scripts/check-new-release-version.sh#L1-L26)
- [CONTRIBUTING.md](file://CONTRIBUTING.md#L1-L135)
- [Makefile](file://Makefile#L244-L270)
- [CHANGELOG.md](file://CHANGELOG.md#L1-L3168)

## Version History
The unstructured library has a detailed version history documented in the CHANGELOG.md file, with releases dating back to the initial 0.2.0 release. The version history shows a pattern of frequent updates with a focus on security, performance improvements, and new features. Recent versions have emphasized security fixes, with multiple CVE patches in versions 0.18.17, 0.18.16, 0.18.15, and 0.18.14. The library has also seen significant feature development, including enhancements to the VoyageAI integration, improved language detection for PDFs, and support for converting elements to markdown format. Performance optimizations have been a consistent theme, with numerous codeflash enhancements that significantly improve function speed. The version history also documents the library's evolution in supporting different file types and partitioning strategies, with recent versions adding support for various document formats and improving the accuracy of element extraction. The library has maintained backward compatibility while deprecating older features and dependencies.

**Section sources**
- [CHANGELOG.md](file://CHANGELOG.md#L1-L3168)

## Breaking Changes and Migration Paths
The unstructured library has implemented several breaking changes throughout its version history, with clear documentation of these changes in the CHANGELOG.md file. One significant breaking change occurred in version 0.15.13, where dead experimental code in `file_utils.experimental` and `file_utils.metadata` was removed, which could potentially break client code that depended on these functions. Another breaking change in version 0.8.0 involved the restructuring of element location data, which was moved from top-level attributes to the `coordinates` attribute of the element's metadata. The library has also deprecated various features over time, such as the `stage_for_label_studio` function in version 0.17.4, which was deprecated due to a CVE in its dependency. Migration paths for these changes are typically provided through the introduction of new APIs or parameters that replace the deprecated functionality. For example, when deprecating the `file_filename` parameter, the library likely provided alternative methods for specifying file metadata. The versioning scheme and clear documentation of breaking changes in the changelog help users plan migrations and understand the impact of upgrading to new versions.

**Section sources**
- [CHANGELOG.md](file://CHANGELOG.md#L667-L684)
- [CHANGELOG.md](file://CHANGELOG.md#L2296-L2298)
- [CHANGELOG.md](file://CHANGELOG.md#L286-L290)

## Upgrade Guides
While the repository does not contain explicit upgrade guide documents, the CHANGELOG.md file serves as a comprehensive upgrade guide by documenting all changes between versions. The changelog categorizes changes into Enhancements, Features, and Fixes, making it easier for users to understand what has changed and how it might affect their usage. For major version transitions, users should carefully review the breaking changes section in the changelog and test their applications with the new version in a staging environment. When upgrading, users should pay particular attention to deprecated features and security fixes. The library's dependency management through pip-compile helps ensure that upgrades include necessary dependency updates. Users should also review the README.md for any updated installation instructions, particularly when adding support for new document types or features. The Makefile and scripts provide tools for checking version consistency, which can be used to validate that the upgrade was successful and that all components are using the expected versions.

**Section sources**
- [CHANGELOG.md](file://CHANGELOG.md#L1-L3168)
- [README.md](file://README.md#L1-L256)
- [Makefile](file://Makefile#L244-L270)

## Backward Compatibility and Deprecation Policies
The unstructured library maintains a strong commitment to backward compatibility while evolving its features and APIs. The project follows a deliberate deprecation policy, typically warning users before removing functionality. For example, in version 0.17.4, the `stage_for_label_studio` function was deprecated rather than immediately removed, allowing users time to migrate to alternative solutions. The library also maintains compatibility with multiple Python versions, currently supporting Python 3.10, 3.11, and 3.12 as indicated in the setup.py file. When backward-incompatible changes are necessary, they are documented as breaking changes in the changelog with clear explanations of the impact. The library's approach to dependency management also reflects its backward compatibility policy, with careful consideration given to dependency updates that might affect existing installations. The project removes deprecated code only after sufficient time has passed for users to migrate, as evidenced by the removal of dead experimental code in version 0.15.13. The library also maintains compatibility with various document formats and processing strategies, ensuring that existing workflows continue to function as expected.

**Section sources**
- [CHANGELOG.md](file://CHANGELOG.md#L286-L290)
- [CHANGELOG.md](file://CHANGELOG.md#L667-L684)
- [setup.py](file://setup.py#L84-L97)

## Long-Term Support and Security Updates
The unstructured library demonstrates a strong commitment to security and long-term support through its regular security updates and vulnerability management. The project actively addresses security vulnerabilities, as evidenced by multiple CVE patches in recent versions, including fixes for path traversal in email MSG attachment filenames (GHSA-gm8q-m8mv-jj5m) in version 0.18.18. The library also proactively updates dependencies to address known CVEs in packages such as pypdf, pip, uv, authlib, and various system libraries. The security policy is documented in the repository, with instructions for reporting security vulnerabilities. The project maintains long-term support by continuing to support older document formats and processing methods while introducing new features. The library's dependency management through pip-compile ensures that security updates to dependencies are regularly incorporated. The project also addresses security in its Docker images by updating base images and removing unnecessary dependencies. The regular release cycle ensures that security fixes are delivered promptly to users, minimizing the window of exposure to known vulnerabilities.

**Section sources**
- [CHANGELOG.md](file://CHANGELOG.md#L15-L16)
- [CHANGELOG.md](file://CHANGELOG.md#L25-L42)
- [SECURITY.md](file://SECURITY.md)
- [Dockerfile](file://Dockerfile)

## Version Increment Criteria
The unstructured library follows Semantic Versioning principles for determining version increments. PATCH versions (e.g., 0.18.19 to 0.18.20) are incremented for backward-compatible bug fixes and security patches, such as the security fix for path traversal in email attachments in version 0.18.18. MINOR versions (e.g., 0.17.x to 0.18.x) are incremented for backward-compatible feature additions and enhancements, such as the improved VoyageAI integration and language detection for PDFs in version 0.18.7. MAJOR versions would be incremented for backward-incompatible changes, though the library is still in the 0.x.y phase, indicating it is under active development. The criteria for version increments are evident in the changelog entries, where new features are typically released in minor versions, while bug fixes and security patches are released as patch versions. Performance improvements and dependency updates are also typically released as patch versions. The project also uses pre-release versions (with the -dev suffix) for development builds, which are not considered stable releases.

**Section sources**
- [CHANGELOG.md](file://CHANGELOG.md#L1-L3168)
- [unstructured/__version__.py](file://unstructured/__version__.py#L1-L2)

## Feature Inclusion Process
The feature inclusion process for the unstructured library is documented in the CONTRIBUTING.md file, which outlines a comprehensive checklist for contributors. Features are typically proposed and discussed through GitHub issues before implementation. The pull request checklist requires contributors to ensure all new classes, functions, and methods have appropriate docstrings and type hints, have associated tests, and update the CHANGELOG.md and version file when core code changes. Features must also pass all automated tests and checks, including linting, formatting, and coverage requirements. The project uses conventional commit messages to standardize PR titles, making it easier to track feature additions in the changelog. Features are often grouped by theme, such as enhancements to specific document type processing or improvements to the partitioning pipeline. The library has a particular focus on expanding support for different document types and improving the accuracy and performance of element extraction. New features are typically introduced in minor versions, with careful consideration given to backward compatibility and user experience.

**Section sources**
- [CONTRIBUTING.md](file://CONTRIBUTING.md#L1-L135)
- [CHANGELOG.md](file://CHANGELOG.md#L1-L3168)