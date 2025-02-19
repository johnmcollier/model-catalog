# RHDH AI Catalog

Reference implementation of an AI catalog in RHDH

## Import

To import into RHDH:

1) Fork this repository

2) Modify ai-catalog.yaml as needed (change urls, change model(s), update techdocs, etc)

3) Copy your URL for ai-catalog.yaml

4) Import into RHDH as an existing Component

## Catalog Structure

In this catalog: 
- Each model server is represented as a `Component` with type `model-server`, containing information such as:
   - Name, description URL, authentication status, and how to get access
- Each model deployed on a model server is represented as a `Resource` with type `ai-model`, containing information such as:
   - Name, description, model usage, intended tasks, tags, license, and author
- An `API` object representing the model server API type (e.g. OpenAI, OpenVINO, etc)
- Each `model-server` Component `dependsOn`:
   - The 1 to N `ai-model` resources deployed on it
   - The `API` object associated with the model server

![AI Catalog](./assets/catalog-graph.png "AI Catalog")


### Model Metadata

The following metadata is stored for each model in the catalog: 

| Name         | Type     | Description | Catalog Implementation |
| ------------ | -------- | ------------| ---------------------- |
| Name         | String     | The name of the model. | Resource `metadata.name` |
| Tasks        | String[]   | The intended tasks for the model. | Resource `metadata.tags[]`. Can prefix task specific tags to highlight e.g. `task-text-generation`, or `task#text-generation`. |
| Usage        | String     | Brief description on the usage for the model. | `metadata.description` |
| Type         | String     | The type of model being stored in the catalog. | Resource techdoc or tags |
| License      | URL        | The license that the model uses. | Resource `metadata.links[]` or techdoc |
| Tags         | String[]   | Descriptive labels for the model to aid in filtering. | Resource `metadata.tags[]` |
| Author       | String     | The author of the model. | Resource `metadata.tags[]` |
| Maintainer   | String     | The maintainer of the model deployed on the model server. | Resource `spec.owner` |
| Instructions | Techdoc    | Instructions on how to access / use the model. | Resource techdocs |
| Download Link | URL         | A link to download the model's files (e.g. GGUF artifacts). | Resource `metadata.links[]` |

### Model Server Metadata

The following metadata is stored for each model server in the catalog: 

| Name                             | Type        | Description                                      | Catalog Implementation |
| -------------------------------- | ----------- | -------------------------------------------------| ---------------------- |
| Model Server Type                | String      | The type of model server API that the model uses. | API |
| Authentication Required?         | Boolean     | Authentication status for the server.             | Component techdocs |
| API Link                         | URL         | A link corresponding to the API endpoint for the model service that has the model deployed. | Component `metadata.links[]` |
| Access Link                         | URL         | A link to access the model if hosted online. | Component `metadata.links[]` |
| API Schema              | String          | The API schema for the model server. | API `spec.definition` |
| Access Instructions     | Techdoc        | How to get access to and how to use the model server. | Component techdoc |



