# MLOPs
Contains MLOPs template. Repo based on the course - https://coursera.org/share/7af235d10a9b127ded8f41e48c345f01.

# commands
file: MLproject
```
# An MLFlow project that has a single entry point to validate a data set
#
name: Dataset Validation

conda_env: conda_env.yml

entry_points:

  main:
    parameters:
      log: {type: bool, default: true}
      max_errors: {type: int, default: 1}
      filename: path
    command: "python validate.py {log} {max_errors} {filename}"
```

run MLproject using -

```mlflow run . -P filename=data.csv```

# project from github
do mlflow run on the ssh link of a repo (need to link instance with ssh key)

``` mlflow run git@github.com:mlflow/mlflow-example.git ```

# project by specifying env

```mlflow run /path/to/conda/project --env-manager virtualenv```

https://mlflow.org/docs/latest/models.html



## Register Models and retrieve them

Registering models is a big step towards reproducibility in ML. Until recently, there was no versioning for datasets or models. MLflow has the ability to register, track, and retrieve models. This aligns MLflow very well with cloud providers like Azure that offers the ability to track models in similar ways.


The basis of model versioning is a [Model Registry](https://mlflow.org/docs/latest/model-registry.html). Model versions are tracked automatically and sequentially. You don't need to specify what the version of a model _is_ in order to save it.

You *must* use `log_model` to register a new model. It isn't possible to add a model without having that done.


### Use an existing model to register it

In this example, we use an [ONNX T5 model](https://github.com/onnx/models/tree/main/text/machine_comprehension/t5) to register it. You can use the `MlflowClient` to create it with the API:

```python
from mlflow import MlflowClient

client = MlflowClient()
client.create_registered_model("onnx-t5")
```


### Retrieving and updating models

You can fetch this model at a specific version with:

```python
model_name = "onnx-t5"
model_version = 1

model = mlflow.pyfunc.load_model(
    model_uri=f"models:/{model_name}/{model_version}"
)

```

Update the model descriptions for a specific version:

```python
client.update_model_version(
    name = "onnx-t5",
    version = 1,
    description = "This is the T5 model in an ONNX version 1.6 using Opset 12"
)
```

### Changing stages

Stages are used as a way to identify if a model is going for a staging environment or going into production. These _stages_ allow you to move models from one to the other and it also affects how to retrieve these models from the registry. There are three stages supported by MLflow:

1. Staging
1. Production
1. Archived

If no stage is specified, it is set to `None`.

Change the stage of a model:

```python
client.transition_model_version_stage(
    name="onnx-t5",
    version=1,
    stage="Production"
)
```
