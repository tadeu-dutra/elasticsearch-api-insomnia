# API Tests: Elasticsearch Sample Collection

This project is a CI/CD automation example focused on interacting with the **Elasticsearch**. It uses the [Insomnia CLI (Inso)](https://developer.konghq.com) and [GitHub Actions](https://docs.github.com) to run automated API regression tests.

For the sake of simplicity and demonstration, this implementation focuses on using just a small portion of the official Elasticsearch API specification, more specifically the [Documents Endpoint](https://www.elastic.co/docs/api/doc/elasticsearch/group/endpoint-document), used to manage documents in an Elasticsearch index. Here is a list of the insomnia requests:

The document APIs enable you to create and manage documents in an Elasticsearch index.

- Get API Keys
- Bulk Operation with versioning
- Bulk Operation without versioning
- Create a new document
- Update a document
- Check a document
- Get multiple documents
- Delete a document

See full spec here: https://www.elastic.co/docs/api/doc/elasticsearch

The workflow checks out the repository, installs the `inso` CLI, injects necessary secrets from the GitHub repository settings, and executes the requests defined in the `TestOps_API_Collection.yaml` file.

## Getting Started

To use this project in your GitHub repository, you will need to configure two main things:

1.  GitHub Repository Secrets
2.  The GitHub Actions Workflow File

### 1. Configure GitHub Secrets

This project requires sensitive information (like your TestOps base URL, API keys, and specific resource IDs) to be stored securely in your GitHub repository's settings. These are used by the CI pipeline at runtime.

Navigate to your repository's **Settings** > **Secrets and variables** > **Actions** > **New repository secret** and add the following secrets:

| Secret Name        | Description                                       |
| ------------------ | ------------------------------------------------- |
| `ES_URL`           | The URL of your elasticsearch instance            |
| `ES_API_KEY`       | Your Katalon API Key                              |
| `ES_INDEX_NAME`    | Target elasticsearch index name                   |
| *... (and any others you use)* | |

### 2. The Workflow File (`.github/workflows/ci.yaml`)

The automation logic is defined in the `ci.yaml` file. The provided configuration performs the following steps on every `push` or `pull_request` to the `main` branch:

*   **Checkout Code:** Clones the repository.
*   **Setup Inso CLI:** installs the required version of the Insomnia CLI.
*   **Run API Collection:** Executes the **API requests** defined in `Elasticsearch_API_Collection.yaml`, injecting the secrets defined above into the `ES_ENVIRONMENT` in the Insomnia context.

You can view the full working configuration in the provided [`ci.yaml`](.github/workflows/ci.yaml) file in this repo.

## Exported Insomnia Data

The `Elasticsearch_API_Collection.yaml` file contains the exported request definitions and environment structure from the Insomnia application. These requests utilize templating (e.g., `{{ _.ES_URL }}`) which are resolved by the secrets at runtime.

NOTE: This file should only contain structure and placeholder variables (e.g., `""`), never actual secrets. The secrets are injected securely via GitHub Actions at runtime.
