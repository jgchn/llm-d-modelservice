# llm-d-modelservice

Helm chart for ModelService.

## Getting started
Add this repository to Helm.

```
helm repo add llm-d-modelservice https://llm-d-incubation.github.io/llm-d-modelservice/
helm repo update
```

See [examples](https://llm-d-incubation.github.io/llm-d-modelservice/charts/llm-d-modelservice/examples) for how to use this Helm chart.

## Values
Below are the values you can set.
| Key                                    | Description                                                                                                       | Type         | Default                                     |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------|--------------|---------------------------------------------|
| `multinode`                            | Determines whether to create P/D using Deployments (false) or LWS (true)                                          | bool         | `false`                                     |
| `inferencePool`                        | If true, creates a InferencePool object                                                                           | bool         | `false`                                     |
| `inferenceModel`                       | If true, creates a InferenceModel object                                                                          | bool         | `false`                                     |
| `httpRoute`                            | If true, creates a HTTPRoute object                                                                               | bool         | `false`                                     |
| `routing.modelName`                    | The name for the `"model"` parameter in the OpenAI request                                                        | string       | N/A                                         |
| `routing.servicePort`                  | The port the routing proxy sidecar listens on. <br>If there is no sidecar, this is the port the request goes to.Ω | int          | N/A                                         |
| `routing.proxy.image`                  | Image used for the sidecar                                                                                        | string       | `ghcr.io/llm-d/llm-d-routing-sidecar:0.0.6` |
| `routing.proxy.targetPort`             | The port the vLLM decode container listens on. <br>If proxy is present, it will forward request to this port.     | string       | N/A                                         |
| `routing.proxy.debugLevel`             | Debug level of the routing proxy                                                                                  | int          | 5                                           |
| `routing.proxy.parentRefs[*].name`     | The name of the inference gateway                                                                                 | string       | N/A                                         |
| `modelArtifacts.uri`                   | Model artifacts URI. Current formats supported include `hf://`, `pvc://`, and `oci://`                            | string       | N/A                                         |
| `modelArtifacts.authSecretName`        | The name of the Secret containing `HF_TOKEN` for `hf://` artifacts that require a token for downloading a model.  | string       | N/A                                         |
| `modelArtifacts.size`                  | Size used to create an emptyDir volume for downloading the model from HF.                                         | string       | N/A                                         |
| `decode.replicas`                      | Number of replicas for decode pods                                                                                | int          | 1                                           |
| `decode.containers[*].name`            | Name of the container for the decode deployment/LWS                                                               | string       | N/A                                         |
| `decode.containers[*].image`           | Image of the container for the decode deployment/LWS                                                              | string       | N/A                                         |
| `decode.containers[*].args`            | List of arguments for the decode container.                                                                       | List[string] | []                                          |
| `decode.containers[*].command`         | List of commands for the decode container.                                                                        | List[string] | []                                          |
| `decode.containers[*].ports`           | List of ports for the decode container.                                                                           | List[Port]   | []                                          |
| `prefill`                              | Same fields supported in `decode`                                                                                 | See above    | See above                                   |
| `endpointPicker.service.permissions`          | Role created for the Inference Scheduler                                  | string       | N/A                                   |
| `endpointPicker.service.type`          | Type of Service created for the Inference Scheduler (Endpoint Picker) deployment                                  | string       | ClusterIP                                   |
| `endpointPicker.service.port`          | The port the Inference Scheduler listens on                                                                       | int          | 9002                                        |
| `endpointPicker.service.targetPort`    | The target port the Inference Scheduler listens on                                                                | int          | 9002                                        |
| `endpointPicker.service.appProtocol`   | The app portocol the Inference Scheduler uses                                                                     | int          | 9002                                        |
| `endpointPicker.replicas`              | Number of replicas for the Inference Scheduler pod                                                                | int          | 1                                           |
| `endpointPicker.debugLevel`            | Debug level used to start the Inference Scheduler pod                                                             | int          | 4                                           |
| `endpointPicker.disableReadinessProbe` | Disable readiness probe creation for the Inference Scheduler pod. <br>Set this to `true` if you want to debug.    | bool         | `false`                                     |
| `endpointPicker.disableLivenessProbe`  | Disable liveness probe creation for the Inference Scheduler pod. <br>Set this to `true` if you want to debug.     | bool         | `false`                                     |