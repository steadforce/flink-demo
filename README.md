# Trino

Repository containing manifests for
[flink jobs](https://nightlies.apache.org/flink/flink-kubernetes-operator-docs-main/docs/custom-resource/reference/)
installation. Never install the content of this repo on our clusters manually. This is all done by argo-cd.

## Dependencies

This chart has no dependencies specified in `Chart.yaml` in the `dependencies` section.
But you need to have the [flink operator](https://github.com/steadforce/flink-operator)
enabled.

## Render helm charts locally

The following command renders the charts like argo-cd does to validate the content.

### local

```
 helm template --release-name flink-demo -n flink-demo --skip-tests \
  -a flink.apache.org/v1beta1 \
  --output-dir _render/local . 
```

You can use this command to check if the output is as you expect. The `-a` parameters are needed since we use the
helm feature `.Capabilities.APIVersions.Has` to determine if a `CR` is installable in the cluster or not. Since
helm templating is designed to work offline we have to list the supported `CR`. Using `.Capabilities.APIVersions.Has`
feature in templating prevents sync errors in argo-cd if a `CR` can't be applied since its `CRD` isn't ready.
