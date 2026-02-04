# credstash-operator Agent Instructions

This document provides instructions for coding agents to work on the `credstash-operator` repository. Following these guidelines will help ensure that changes are made efficiently and correctly.

## High-Level Details

*   **Summary:** `credstash-operator` is a Kubernetes operator that creates Kubernetes secrets from secrets stored in `credstash`. It watches for `CredstashSecret` custom resources and manages the lifecycle of the corresponding Kubernetes `Secret`.
*   **Project Type:** Kubernetes Operator
*   **Language:** Go
*   **Frameworks/Libraries:**
    *   [Kubernetes Operator SDK](https://sdk.operatorframework.io/)
    *   [Cobra](https://github.com/spf13/cobra) (indirectly via client-go)
    *   [controller-runtime](https://github.com/kubernetes-sigs/controller-runtime)
*   **Target Runtimes:** Kubernetes

## Build Instructions

The following commands are used to build, test, and lint the project. These are the authoritative commands and should be used to validate any changes.

### Setup

Before building or testing, you may need to install the required dependencies. The `setup` make target will install the necessary Go tools.

```bash
make setup
```

### Building

To build the `credstash-operator` binary, use the `build` or `build-cross` make target. `build-cross` is the default and builds for multiple platforms.

```bash
make build
```

This will create the binaries in the `_dist` directory.

### Testing

To run the unit tests, use the `test` make target.

```bash
make test
```

To generate a test coverage report, use the `cover` make target.

```bash
make cover
```

### Linting

To lint the Go code, use the `lint` make target. This uses `golangci-lint` with a specific configuration.

```bash
make lint
```

### Formatting

To format the Go code, use the `fmt` make target.

```bash
make fmt
```

### Generating Code

The project uses code generation for Kubernetes resources. To regenerate the code, use the `generate` make target.

```bash
make generate
```

### Docker Images

To build the Docker image, use the `docker-build` make target. This requires `gcloud` to be configured.

```bash
make docker-build
```

To push the Docker image, use the `docker-push` make target.

```bash
make docker-push
```

### Helm Chart

To package the Helm chart, use the `helm-package` make target.

```bash
make helm-package
```

To lint the Helm chart, use the `helm-lint` make target.

```bash
make helm-lint
```

## Project Layout

The repository is structured as a standard Kubernetes Operator SDK project.

*   `cmd/manager/main.go`: The main entrypoint for the operator.
*   `pkg/apis/`: Contains the API definitions for the `CredstashSecret` custom resource.
    *   `pkg/apis/credstash/v1alpha1/`: The v1alpha1 version of the API.
        *   `credstashsecret_types.go`: The Go types for the `CredstashSecret` CRD.
*   `pkg/controller/`: Contains the implementation of the controller.
    *   `pkg/controller/credstashsecret/credstashsecret_controller.go`: The core reconciliation logic for the `CredstashSecret` controller.
*   `deploy/`: Contains deployment manifests for the operator.
    *   `deploy/crds/`: The Custom Resource Definition (CRD) files.
    *   `deploy/helm/`: The Helm chart for deploying the operator.
*   `build/`: Contains Dockerfile and cloud build configurations.
*   `Makefile`: The main build file with all the commands for building, testing, and linting.
*   `go.mod`: The Go module definition file.
*   `go.sum`: The Go module checksum file.

## Validation and CI

The project uses a `.prow.yaml` file for CI, which is not fully visible, but the `Makefile` contains all the necessary commands to build and validate the project. The primary validation steps are:

1.  `make fmt`
2.  `make lint`
3.  `make test`
4.  `make build`

Any pull request should pass these checks.

## Agent Instructions

*   Trust the instructions in this document.
*   Use the `make` commands provided to build, test, and lint your changes.
*   If you encounter an issue that is not covered by this document, you can search the codebase, but prefer to use the commands and information provided here first.
*   When making changes to the API definitions in `pkg/apis/`, remember to run `make generate` to update the generated code.
*   For any change, ensure that the existing tests pass and add new tests as necessary.
