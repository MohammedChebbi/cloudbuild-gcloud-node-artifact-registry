# cloudbuild-gcloud-node-artifact-registry
Cloud build file for publishing node dependency in gcp artifact registry

steps:
  - name: gcr.io/cloud-builders/gcloud
    entrypoint: bash
    env:
      - NODE_ENV=PRODUCTION
    args:
      - "-c"
      - |
        gcloud artifacts print-settings npm \
        --project=projectx \
        --repository=repox \
        --location=europe-west1 \
        --scope=@bots > .npmrc
  - name: node:$_NODE_VERSION
    entrypoint: bash
    args:
      - "-c"
      - |
        npx google-artifactregistry-auth

        npm install
        npm run build
        npm publish
