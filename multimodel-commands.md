helm upgrade --install first-model charts/llm-d-modelservice -f examples/values-cpu.yaml --set routing.parentRefs[0].name=inference-gateway-inference-gateway --set prefill.replicas=1 --set routing.proxy.image=ghcr.io/llm-d/llm-d-routing-sidecar:v0.2.0@sha256:a623a0752af0a71b7b05ebf95517848b5dbc3d8d235c1897035905632d5b7d80  --set routing.inferenceModel.create=true --set routing.proxy.secure=false


k port-forward svc/inference-gateway-inference-gateway 8000:80


curl http://localhost:8000/v1/completions -vvv \
        -H "Content-Type: application/json" \
        -H "x-model-name: random/model" \
        -d '{
        "model": "random/model",
        "prompt": "Hello, "
    }'

curl http://localhost:8000/v1/completions


helm upgrade --install third-model charts/llm-d-modelservice -f examples/values-cpu.yaml --set routing.parentRefs[0].name=inference-gateway-inference-gateway --set prefill.replicas=1 --set routing.proxy.image=ghcr.io/llm-d/llm-d-routing-sidecar:v0.2.0@sha256:a623a0752af0a71b7b05ebf95517848b5dbc3d8d235c1897035905632d5b7d80  --set routing.inferenceModel.create=true --set routing.proxy.secure=false

helm delete second-model
