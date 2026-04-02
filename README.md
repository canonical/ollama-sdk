# Ollama SDK for Workshop

A runtime environment for serving large language models locally. It ships
the Ollama binary, runs the server as a user service, and persists downloaded
models on the host across workshop updates.

---

## Reference workshop

A minimal workshop:

```yaml
# workshop.yaml
name: ollama-app
base: ubuntu@24.04
sdks:
  - name: ollama
    channel: latest/stable

actions:
  pull: |
    ollama pull "$@"
  run: |
    ollama run "$@"
```

This demonstrates pulling and running a model with persistent model storage.

---

## Using the SDK

### Prerequisites, project layout

1. No prerequisite SDKs are required.
2. No project files are needed; the SDK provides the Ollama server and CLI.
3. On launch, the SDK starts the Ollama server as a systemd user service and
   configures `PATH`.

### Pull and run a model

Once the workshop is ready:

```bash
workshop shell
ollama pull llama3.2
ollama run llama3.2
```

Downloaded models are stored in `~/.ollama/models`, which is mapped to the host
via the `models` mount plug. Subsequent workshop updates reuse cached models.

### Use the API

The Ollama server exposes a REST API on port 11434 inside the workshop. Other
SDKs can reach it via the `ollama-server` tunnel slot:

```bash
curl http://localhost:11434/api/generate -d '{
  "model": "llama3.2",
  "prompt": "Hello"
}'
```

---

## Plugs (resources this SDK consumes)

### `gpu`

- Interface: `gpu`
- Purpose: Grants access to GPU hardware on the host for model inference.

### `models`

- Interface: `mount`
- Workshop target: `/home/workshop/.ollama/models`
- Purpose: Persists downloaded models between workshop updates.

## Slots (resources this SDK provides)

### `ollama-server`

- Interface: `tunnel`
- Endpoint: `11434`
- Purpose: Exposes the Ollama API server for use by other SDKs or host tools.

---

## Documentation and guidance

- [Ollama official documentation](https://github.com/ollama/ollama/blob/main/README.md)
- [Ollama API reference](https://docs.ollama.com/api)
- [Workshop documentation](https://canonical-workshop.readthedocs-hosted.com/latest/)

---

## Community and support

- Ollama community: [Ollama Discord](https://discord.com/invite/ollama)
- Workshop forum:
  [Workshop Discourse](https://discourse.canonical.com/c/engineering/workshops/34)
- Please review our
  [Code of Conduct](https://ubuntu.com/community/ethos/code-of-conduct) before
  participating.

---

## Contributions

All contributions, including code, documentation updates, and issue reports,
are welcome!

- See `CONTRIBUTING.md` for guidelines.
- Open issues or pull requests on the official repository.

---

## License and copyright

Copyright 2025 Canonical Ltd.

This SDK is licensed under the
[MIT License](https://opensource.org/licenses/MIT), the same license as
[Ollama](https://github.com/ollama/ollama/blob/main/LICENSE).
