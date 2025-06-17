# A Quick Guide To Deploy Shopify's ToxyProxy via Docker. 

## Steps (using Docker )
1. Install Toxy CLI 
```bash
 docker pull ghcr.io/shopify/toxiproxy

```

2. Configure External network binding for Toxy container using TOXIPROXY_HOST
```bash
docker run --rm -it \
  -e TOXIPROXY_HOST=0.0.0.0 \
  -p 8474:8474 \
  ghcr.io/shopify/toxiproxy
 ```

double check if this returns lines containing ` "level":"info","address":"0.0.0.0:8474","caller":"api.go:57","time":"2025-06-16T00:50:00Z","message":"Starting Toxiproxy HTTP server"} `


3. Two options for configuring proxy
    - Polulating ToxiProxy via -config or mounting to config/toxiproxy.json use [1] format
    - Via Toxy CLI, see example in [1]



## Helpers

1. Docker rebuild
```bash
docker-compose down -v
docker-compose up --build --force-recreate
```

2. Quick note, to display list of proxy via CLI:

```bash 
toxiproxy-cli --host=localhost:8474 list  
```
3. To check if Toxic has been applied:

```bash 
curl http://localhost:8474/proxies 
```

4. Toxic must be applied through HTTP method:

```bash 
curl -X POST http://localhost:8474/proxies/redis_proxy/toxics \
  -H "Content-Type: application/json" \
  -d '{
    "name": "latency_sim",
    "type": "latency",
    "stream": "upstream",
    "toxicity": 1.0,
    "attributes": {
      "latency": 5000

    }
  }'


```

## Reference 
[1] Shopify, “GitHub - Shopify/toxiproxy: :alarm_clock: A TCP proxy to simulate network and system conditions for chaos and resiliency testing,” GitHub. https://github.com/Shopify/toxiproxy?tab=readme-ov-file#cli-example

Credit for ToxyProxy by Shopify: https://github.com/Shopify/toxiproxy