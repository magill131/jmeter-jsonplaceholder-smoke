# JMeter JSONPlaceholder Smoke Test

A tiny Apache JMeter test plan against the free public demo API [JSONPlaceholder](https://jsonplaceholder.typicode.com).

## What it does

5 virtual users ramp up over 5 seconds and each runs the flow twice (10 iterations, 30 requests total):

| Request | Checks |
| --- | --- |
| `GET /posts/1` | status 200, `$.id` equals 1, response under 2 seconds |
| `GET /posts?userId=1` | status 200, first item's `userId` equals 1 |
| `POST /posts` | status 201, `title` is echoed back |

Note: JSONPlaceholder fakes writes. The POST returns a created response but nothing is saved.

## Run it

Open `jsonplaceholder-smoke.jmx` in the JMeter GUI, or run headless:

```bash
jmeter -n -t jsonplaceholder-smoke.jmx -l results.jtl -e -o report
```

Open `report/index.html` for the HTML dashboard.

## CI

`.github/workflows/jmeter.yml` downloads JMeter, runs the plan headless on every push, fails the job if any request fails an assertion, and uploads the results and HTML report as an artifact.

## Scope

This is a smoke test with light load, not a load or stress test. Be polite to the public API and keep thread counts low.
