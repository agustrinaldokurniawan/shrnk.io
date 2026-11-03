# shrnk.io

Short links in, click counts out. Links expire in seven days.

_Branch `main` — v1 · [design](docs/design.md) · [live](url)_

## The story

Two short paragraphs. What the thing does, then the part that fought back — the redirect
has to answer fast, the click still has to be counted, and a cache makes the click vanish —
and the number that settled it.

## Points

- p99 < 20 ms, with every click still recorded
- <n>× headroom after one change: <what it was>
- $<n>/mo at <n> links, one machine
- No accounts, no dashboard — the design says what each removal saved

## API

[![API reference](docs/img/api.png)](<public doc url>)

Generated from `openapi.yaml`, published with <Scalar / Postman / Redoc>. The screenshot is
the entry point; the link is the full thing.

## Monitoring

[![Grafana](docs/img/grafana.png)](<dashboard url>)

<One line: what this dashboard proves — p99, error rate, click queue depth.>

## Run

```bash
cp .env.example .env && docker compose up
```

## License

MIT
