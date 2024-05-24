# ouranos-ws

## Getting Started

### Prerequisites

- [Debian](https://www.debian.org)
- [Docker Engine](https://docs.docker.com/engine/install/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [Certbot](https://certbot.eff.org/instructions?ws=other&os=debianbuster)

### Installation

- Create type A entries for the following subdomains:
    - api.ouranos-ws.example.com
    - app.ouranos-ws.example.com
    - data-models.ouranos-ws.example.com
    - idm.example.com

- Generate HTTPS certificates
    - `sudo certbot certonly --standalone -d api.ouranos-ws.example.com`
    - `sudo certbot certonly --standalone -d app.ouranos-ws.example.com`
    - `sudo certbot certonly --standalone -d data-models.ouranos-ws.example.com`
    - `sudo certbot certonly --standalone -d idm.example.com`

- Create and edit the docker compose environment file
    ```
    cp .env.example .env
    ```

- Create and edit the NGINX config file
    ```
    cp nginx.conf.example nginx.conf
    ```
