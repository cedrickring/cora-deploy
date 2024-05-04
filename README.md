# Cora Deployment

This repository contains the necessary files to run the Cora application using Docker and Docker Compose.

## Pre-requisites

Make sure to have the following tools installed:
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Configuring the application

After cloning the repository, the application can be configured by altering the .env file in this repo. To create a new .env file, copy the .env.sample file and adjust the values accordingly.

### OpenAI
For OpenAI you must set the following environment variables accordingly:

```shell
OPENAI_API_BASE="https://api.openai.com/v1"
OPENAI_API_KEY="<YOUR_API_KEY>"
OPENAI_EMBEDDINGS_MODEL="<YOUR_EMBEDDINGS_MODEL>"
OPENAI_GPT_MODEL="<YOUR_GPT_MODEL>"
```

### Azure OpenAI
For Azure OpenAI you must set the following environment variables accordingly:

```shell
OPENAI_API_BASE="<YOUR_ENDPOINT>"
OPENAI_API_KEY="<YOUR_API_KEY>"
OPENAI_EMBEDDINGS_MODEL="<YOUR_EMBEDDINGS_MODEL>"
OPENAI_GPT_MODEL="<YOUR_GPT_MODEL>"

# Azure specific variables
OPENAI_API_TYPE='azure'
AZURE_EMBEDDINGS_DEPLOYMENT="<YOUR_EMBEDDINGS_DEPLOYMENT>"
AZURE_GPT_MODEL_DEPLOYMENT="<YOUR_GPT_MODEL_DEPLOYMENT>"
```

## Preprocessing the data

Before running the application, you need to preprocess the documents you want to ask questions against. The documents need to be put in `input_files/`. For now, we only support `.pdf` files.

To preprocess the documents, run the following command:
```bash
docker compose run preprocessing
```

This will preprocess the documents and store the results in the `vector_databases/` directory. As soon as the process is complete, you can continue with the next step: [Running the application](#running-the-application).

**NOTE**: This process can take a few minutes depending on the amount and the size of the documents to process.

## Running the application

This repository contains an automatically updated `compose.yaml` file that will pull the latest version of the Cora application and run it.
In addition to the autoconfiguration, you need to manually specify the environment variables specified in the `.env.sample` file. You can do this by copying the `.env.sample` file to `.env` and adjust the values accordingly.
```bash
cp .env.sample .env
```

After setting the environment variables, you can run the application using the following command:
```bash
docker compose up -d
```

This will start the application and make it accessible through a reverse proxy (nginx) at `http://localhost`. As this setup will probably not satisfy your needs, feel free to adjust the `nginx.conf` file.

## Stopping the application

To stop the application, run the following command:
```bash
docker compose down
```
