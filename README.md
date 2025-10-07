# Open WebUI + vLLM
A simple self-hosted setup to get Open WebUI running with vLLM for inference (instead of Ollama GARB).

The app is served through tailscale.
tailscale si magic


### installation, a concise and incomplete setup guide

Copy.

I keep mine in `/srv/selfhosted/apps/openwebui-vllm/`.

```sh
docker compose pull
docker compose up -d
```

Check it out:
```sh
docker compose ps

NAME                          IMAGE                                COMMAND                  SERVICE      CREATED         STATUS                            PORTS
openwebui-vllm-caddy-1        caddy:2                              "caddy run --config …"   caddy        8 seconds ago   Up 7 seconds                      80/tcp, 443/tcp, 2019/tcp, 443/udp, 127.0.0.1:2080->2080/tcp
openwebui-vllm-open-webui-1   ghcr.io/open-webui/open-webui:main   "bash start.sh"          open-webui   8 seconds ago   Up 8 seconds (health: starting)   8080/tcp
openwebui-vllm-vllm-1         vllm/vllm-openai:latest              "python3 -m vllm.ent…"   vllm         8 seconds ago   Up 8 seconds                      8000/tcp

```

*Noyce.*

vllm takes a while to download a model if its your first time using a particular model.
You can check if that puppy's ready to rip if you see something like:
`INFO:     Application startup complete.`

from the docker logs, e.g.:
```sh
docker logs --tail=5 openwebui-vllm-vllm-1
(APIServer pid=1) INFO 10-07 07:23:54 [launcher.py:42] Route: /invocations, Methods: POST
(APIServer pid=1) INFO 10-07 07:23:54 [launcher.py:42] Route: /metrics, Methods: GET
(APIServer pid=1) INFO:     Started server process [1]
(APIServer pid=1) INFO:     Waiting for application startup.
(APIServer pid=1) INFO:     Application startup complete.
```
hell yeah.

Now get tailscale pointed to that sucker:
```sh
tailscale reset
tailscale serve --bg --https=443 http://127.0.0.1:2080
```

Check it's connected with reverse proxy:
```sh
tailscale serve status
https://megaswol-doge.tail1cf2cf.ts.net (tailnet only)
|-- / proxy http://127.0.0.1:2080
```

Good to go.
Send it 👌🏻

----


## Note
There was an unbelievable amount of bullshit to get this mf to a working state.
I don't know anything about self-hosting or network stuff.
This setup was reached by just hitting a lot of brick walls with my face.

**This readme was written for my reference.**

So, many assumptions, most of which I do not recall, but some of the big boyes are:

- you are a free and libre person
  - but use proprietary nvidia drivers with your nvidia cards
- you've got your cuda and all them deps dialed
- you've installed docker, docker compose
- you've done whatever that nvidia-container toolkit thing
- tailscale is good to go
- you can eat shit for days, have all progress and working memory obliterated several times, and still persist and get it running
