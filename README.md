# MLOPs
Contains MLOPs template. Repo based on the course - https://coursera.org/share/7af235d10a9b127ded8f41e48c345f01.


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
