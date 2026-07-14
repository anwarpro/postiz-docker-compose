
## Watch the Tutorial for docker-compose install:
[https://m.youtube.com/watch?v=A6CjAmJOWvA&t=5s](https://m.youtube.com/watch?v=A6CjAmJOWvA&t=5s)

## Warning
If you are upgrading from Postiz old version, please make sure you update your docker compose, you can read more here:
https://docs.postiz.com/installation/migration

### Configuration uses environment variables

The docker containers for Postiz are entirely configured with environment variables.

- **Option A** - environment variables in your `docker-compose.yml` file
- **Option B** - environment variables in a `postiz.env` file mounted in `/config` for the Postiz container only
- **Option C** - environment variables in a `.env` file next to your `docker-compose.yml` file (not recommended).

... or a mixture of the above options!

There is a [configuration reference](https://docs.postiz.com/configuration/reference) page with a list
of configuration settings.

Setup:
```
git clone https://github.com/gitroomhq/postiz-docker-compose
```

Then run:
```
docker compose up
```

Wait for it to load:

Open your website on https://localhost:4007

## Coolify deployment

This fork's `docker-compose.yaml` is ready for Coolify. It does not publish host ports;
Coolify should route traffic to the `postiz` service on port `5000`.

Use these Coolify settings:

```text
Service: postiz
Port: 5000
Domain: http://postiz.helloanwar.com
```

Add these required environment variables in Coolify before deploying:

```env
POSTIZ_URL=https://postiz.helloanwar.com
POSTIZ_BACKEND_URL=https://postiz.helloanwar.com/api
POSTIZ_JWT_SECRET=replace-with-a-long-random-secret
POSTIZ_POSTGRES_PASSWORD=replace-with-a-strong-postgres-password
POSTIZ_DISABLE_REGISTRATION=false
```

Temporal uses an internal Docker-network-only PostgreSQL database with a fixed
alphanumeric password in `docker-compose.yaml`. It is not published outside the
compose network.

After creating your first account, change this and redeploy:

```env
POSTIZ_DISABLE_REGISTRATION=true
```

When using a system-installed Cloudflare Tunnel, configure the public hostname
to route to Coolify's HTTP proxy, not directly to the Postiz container:

```text
postiz.helloanwar.com -> http://localhost:80
```

Keep the Coolify domain as `http://postiz.helloanwar.com` for tunnel mode so
Coolify does not force HTTPS or request a Let's Encrypt certificate behind the
Cloudflare Tunnel.
