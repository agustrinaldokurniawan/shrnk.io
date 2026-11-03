# shrnk.io

Short links in, a redirect out. Every link dies after seven days.

## Why it is not a twenty-line project

A URL shortener looks like a lookup table until you put it under a number. The redirect has
to answer in under 20 ms, and expiry has to be decided while that lookup is happening — no
background sweeper, no nightly job, just a comparison on the hot path. On two vCPUs that
leaves very little room for anything clever.

So this is a straightforward system measured honestly: one service, one database, an index
that serves exactly one query, and a p99 that either holds at 400 rps or does not.

## Points

- p99 < 20 ms on the redirect at 400 rps, on 2 vCPU
- 2.5M links on a 500 MB database, expiring after 7 days
- One service, one table, one index — no cache, because the numbers say CPU binds first
- No accounts, no dashboard, no aliases; the design lists what each removal saved

## API

| method | path      | result                                            |
| ------ | --------- | ------------------------------------------------- |
| POST   | `/url`    | `201` short link · `400` malformed or non-http(s) |
| GET    | `/{code}` | `302` · `404` unknown · `410` expired             |

Full shapes and error bodies: [docs/design.md](docs/design.md#3--api).

## Run

```bash
cp .env.example .env
docker compose up
```

```bash
curl -X POST localhost:8080/url -H 'content-type: application/json' \
  -d '{"original_url":"https://example.com"}'

curl -i localhost:8080/<code>
```

## Numbers

|                        |                        |
| ---------------------- | ---------------------- |
| hardware               | 2 vCPU / 8 GB / 500 MB |
| p50 / p99 @ 400 rps    | `<measure>`            |
| max rps at p99 < 20 ms | `<measure>`            |
| $/mo                   | `<measure>`            |

Measured, not estimated. The arithmetic behind the 400 rps target is in the design.

## License

MIT
