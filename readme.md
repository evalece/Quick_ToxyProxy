# send RESP command to proxy using a container. 


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

## Reference 
[1] https://github.com/Shopify/toxiproxy?tab=readme-ov-file#cli-example

4. Quick note, to display list of proxy via CLI:

```bash 
toxiproxy-cli --host=localhost:8474 list  
```


Helpers: 

1. Docker rebuild
bash```
docker-compose down -v
docker-compose up --build --force-recreate
```