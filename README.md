# Swagger Editor Validate Github Action

This GitHub Actions validates OpenAPI (OAS) definition file using Swagger Editor.

## Inputs

### `swagger-editor-url`

**Optional** Defines URL of [swagger-editor](https://www.npmjs.com/package/swagger-editor) where definition
file is going to be validated. Default `https://editor.swagger.io/`.

### `definition-file`

**Required** Defines path of [OAS](https://github.com/OAI/OpenAPI-Specification) definition file that exists
as a physical file in your repository.


## Example usage

There are two major use-cases of how to use this GitHub Action.

### Public use-case

If you have access to the internet and don't mind that this action sends your OAS definition
to https://editor.swagger.io/ for validation.

```yaml
on: [push]

jobs:
  test_swagger_editor_validator_remote:
    runs-on: ubuntu-latest
    name: Swagger Editor Validator Remote

    steps:
      - uses: actions/checkout@v2
      - name: Validate OpenAPI definition
        uses: char0n/swagger-editor-validate@v1
        with:
          definition-file: examples/openapi-2-0.yaml
```

### Private use-case

If you want to maintain complete privacy and your OAS definition may contain
sensitive information use the following workflow. The workflow uses swagger-editor
docker image that runs as service of the workflow.

```yaml
on: [push]

jobs:
  test_swagger_editor_validator_service:
    runs-on: ubuntu-latest
    name: Swagger Editor Validator Service

    # Service containers to run with `runner-job`
    services:
      # Label used to access the service container
      swagger-editor:
        # Docker Hub image
        image: swaggerapi/swagger-editor
        ports:
          # Maps port 8080 on service container to the host 80
          - 80:8080

    steps:
      - uses: actions/checkout@v2
      - name: Validate OpenAPI definition
        uses: char0n/swagger-editor-validate@v1
        with:
          swagger-editor-url: http://localhost/
          definition-file: examples/openapi-2-0.yaml
```