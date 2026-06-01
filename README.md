# ObservAI

**An LLM agent that investigates observability incidents.**

> Status: 🚧 Work in progress — built in the open as a 6-week project.

## What it does

ObservAI takes an observability alert as input (e.g. `503 errors on checkout-api +500%`),
runs an agent that investigates the incident — reading logs, traces, metrics and runbooks —
and returns a **diagnosis** and a **recommended action**.

It is a learning-grade reference implementation of an LLM agent for incident response,
with a focus on the engineering around the model: retrieval, tool use, evaluation,
guardrails, deployment and observability of the agent itself.

## Architecture (planned)

```
Alert ──▶ FastAPI service ──▶ Agent loop (ReAct)
                                 │
                                 ├─ search_logs()      ┐
                                 ├─ get_traces()       ├─ tools over mocked observability data
                                 ├─ query_deploys()    ┘
                                 └─ RAG over runbooks (pgvector, hybrid search)
                                 │
                                 ▼
                          Diagnosis + recommendation
```

The whole service is containerised (Docker), deployed locally on Kubernetes (minikube),
and instrumented with OpenTelemetry. LLM-level observability is captured via Datadog.

## Tech stack (planned)

- **LLM / agent:** Anthropic Claude SDK (tool use), ReAct loop
- **Retrieval:** pgvector, hybrid search (BM25 + dense embeddings)
- **API:** FastAPI, Pydantic
- **Eval:** golden set + LLM-as-a-judge, run in CI (GitHub Actions)
- **Infra:** Docker, Kubernetes (minikube), OpenTelemetry, Datadog

## Roadmap

- [ ] **W3** — RAG over runbooks (chunking, embeddings, `/search`, precision@5)
- [ ] **W4** — FastAPI service + Docker, mocked observability dataset
- [ ] **W5** — ReAct agent loop with 3 tools, `/investigate` endpoint
- [ ] **W6** — Evals, guardrails, CI
- [ ] **W7** — Kubernetes manifests, deploy on minikube
- [ ] **W8** — OpenTelemetry instrumentation, LLM observability, write-up

## License

MIT — see [LICENSE](LICENSE).