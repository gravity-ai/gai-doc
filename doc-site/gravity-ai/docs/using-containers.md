# Integrating Production Containers

Production ready models all have a standard API interface. They are supplied as part of a Docker Container.

## Running the container locally

Models are downloaded as container images that need to be imported into your docker repository. After the download completes, you can import it into your local docker repository with the following command (replace 'model_name_123abc.tar.gz' with the actual file name):

```
docker load -i .\model_name_123abc.tar.gz
```

If the import is successful you should see a message similar to this:

```
Loaded image: gx-images:t-12345678abcd1234abcd12345678abcd
```

Use that image tag name to start and run your container:

```
docker run -d -p 9090:80 gx-images:t-12345678abcd1234abcd12345678abcd
```

You should now be able to access the container at `http://localhost:9090`.

<i>Note: This example uses port 9090. Change this value to your desired port number for deployment.</i>

## Using Volumes to Persist Data

If you want to persist data for a container (or share the job queue amongst multiple containers) you need to create a docker volume. (Example shows creating a volume with name 'data-vol')

```
docker volume create data-vol
```

When you run the container, you need to bind this volume to the container using the --mount option:

```
docker run -d -p 9090:80 --mount source=data-vol,target=/var/gai_data gx-images:t-12345678abcd1234abcd12345678abcd
```

<b>Important:</b> If you are using different models from gravity ai, each type of model should use a different volume. Sharing a volume between different model containers is not supported and may cause errors and data corruption.

# Submitting Data

<i>Note: Before data can be submitted, you may need to apply a [license key](/license-keys) to the container</i>

Production Models container a Swagger Endpoint to describe the api. this can be accessed at `http://localhost:9090/swagger`.

Data can be uploaded manually using the container GUI at `http://localhost:9090/AddJob`, or POST to the api endpoint '`http://localhost:9090/data/add-job`.

## Mapping Data

An optional [mapping file](mapping-file.md) may be supplied when submitting structured data. This is typically used to transform data that doesn't follow the exact schema used by the model. It allows the fields from the source data to map to the fields used by the model. If you have previously tested data against this model in the marketplace, a mapping file can be downloaded from your test results page. It can also be created manually.
