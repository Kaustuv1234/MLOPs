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
```mlflow run /path/to/conda/project --env-manager virtualenv
```
