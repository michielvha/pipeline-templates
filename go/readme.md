# Go Pipeline Template

A reusable multi-stage Azure DevOps pipeline template for building and releasing Go applications.

[[_TOC_]]

## Overview

This template provides a complete CI/CD pipeline for Go applications with the following features:
- Code quality checks (linting)
- Binary build and artifact publishing
- Container build and publishing (optional)
- Proper versioning and tagging

## How to Use

Add this template to your pipeline configuration:

```yaml
stages:
- template: pipelines/go-release-stages.yml@pipelineTemplates
  parameters:
    projectName: <YOUR_PROJECT>        # Required parameter
    # All other parameters are optional with sensible defaults
```

## Stages

### 1. Code Quality
- **Purpose**: Validates code quality through linting
- **Features**:
  - Automatically detects and uses `.golangci.yml` config
  - Continues on lint failures based on configuration

### 2. Binary Build and Publish
- **Purpose**: Builds the Go binary and publishes artifacts
- **Features**:
  - Uses goreleaser for consistent builds
  - Uploads artifacts to JFrog Artifactory
  - Uses PGP signing for security

### 3. Container Build and Publish
- **Purpose**: Creates and publishes Docker images
- **Features**:
  - Conditional execution (typically only on main branch)
  - Uses Docker buildx for optimized builds
  - Adds appropriate metadata and tags

## Key Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `projectName` | **Required**: Name of your Go project | (none) |
| `goVersion` | Go runtime version | `1.24.2` |
| `goReleaserVersion` | GoReleaser version | `v2.4.7` |
| `clean` | Whether to clean workspace | `false` |
| `owner` | Owner of the project | `edgeforge` |
| `poolName` | Azure DevOps agent pool | `default` |
| `dockerCondition` | Condition for Docker build stage | `eq(variables['Build.SourceBranchName'], 'main')` |
| `AllowLintFailure` | Whether to continue on linting errors | `false` |

## Requirements

- Requires a Dockerfile in the repository root
- GoReleaser configuration in the repository
- JFrog Artifactory integration for artifact storage
- Azure DevOps pipeline setup with access to the repository
