# Try Vicuna on your own!

THIS REPOSITORY IS NOT INTENDED TO BE USED BY YOU.
USE AT YOUR OWN RISK.

## Usage

### Create a GCS bucket

```sh
gcloud storage buckets create gs://${BUCKET_NAME}/ --location us-west1
```

### Submit a generation job

`yq` is expected to be installed in order to create `job.json`.

```sh
export BUCKET_NAME=${BUCKET_NAME}
yq eval -o json '.taskGroups[0].taskSpec.volumes[0].gcs.remotePath = env(BUCKET_NAME)' generate-vicuna-13b-hf.job.yaml \
  | gcloud batch jobs submit generate-vicuna-13b-hf --location us-central1 --config -
```

You may need to enable Batch API, etc. in advance.

```sh
gcloud services enable batch.googleapis.com compute.googleapis.com logging.googleapis.com
```

### Submit a conversion & quantization job

```sh
yq eval -o json '.taskGroups[0].taskSpec.volumes[0].gcs.remotePath = env(BUCKET_NAME)' convert-vicuna-13b-hf-to-q4_2.job.yaml \
  | gcloud batch jobs submit convert-vicuna-13b-hf-to-q4-2 --location us-central1 --config -
```

### Generation

You can now generate words with the generated bin file by [llama.cpp](https://github.com/ggerganov/llama.cpp).
