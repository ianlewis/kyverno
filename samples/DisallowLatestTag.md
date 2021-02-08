# Disallow latest image tag

The `:latest` tag is mutable and can lead to unexpected errors if the upstream image changes. A best practice is to use an immutable tag that maps to a specific and tested version of an application image.

## Policy YAML

[disallow_latest_tag.yaml](best_practices/disallow_latest_tag.yaml)

````yaml
apiVersion : kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: disallow-latest-tag
spec:
  validationFailureAction: audit
  rules:
  - name: require-image-tag
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: "An image tag is required"  
      pattern:
        spec:
          containers:
          - image: "*:*"
  - name: validate-image-tag
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: "Using a mutable image tag e.g. 'latest' is not allowed"
      pattern:
        spec:
          containers:
          - image: "!*:latest"
````