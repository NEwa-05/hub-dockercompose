# hub-dockercompose

Testing HUB API Gateway on docker machine without Swarm

Question was to push the token outside of the service configuration.

## Deploy Hub

### Create an environment file with the token

```bash
echo -e "HUB_TOKEN=${token_generated_through_hub_dashboard}" > hub.env
```

### Create hub compose with

```bash
cat << EOF > hub.yaml
version: '3.8'

services:
  hub:
    env_file: 
      - hub.env
    image: ghcr.io/traefik/traefik-hub:v3
    command:
      - --providers.docker
      - --hub.token=$HUB_TOKEN

    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    ports:
      - "0.0.0.0:80:80"
      - "0.0.0.0:8080:8080"
EOF
```

### Start Hub

```bash
docker-compose -f hub.yaml up
```
