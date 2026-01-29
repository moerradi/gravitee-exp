# Gravitee API Management with Nginx Reverse Proxy

This setup includes Gravitee API Management platform with an nginx reverse proxy that places all front-facing components behind it.

## Architecture

- **Nginx Reverse Proxy**: Acts as the single entry point for all requests
- **Gravitee Gateway**: API Gateway service (internal port 8082)
- **Management API**: Management API service (internal port 8083)
- **Management UI**: Console web interface (internal port 8080)
- **Portal UI**: Developer portal web interface (internal port 8080)
- **Alert Engine**: Alerts and notifications service (internal port 8072)
- **MongoDB**: Database for configuration and analytics
- **Elasticsearch**: Search engine for analytics
- **Redis**: In-memory data store for rate limiting and caching

## Access URLs

All services are now accessible through nginx on port 80:

- **Developer Portal**: http://localhost/portal/
- **Management Console**: http://localhost/console/
- **API Gateway**: http://localhost/api/
- **Management API**: http://localhost/management/

## Quick Start

1. Start the services:
   ```bash
   docker-compose up -d
   ```

2. Wait for all services to be ready (check with `docker-compose logs`)

3. Access the Developer Portal at http://localhost/portal/

4. Access the Management Console at http://localhost/console/

## SSL/HTTPS Setup

To enable HTTPS:

1. Place your SSL certificates in the `ssl/` directory:
   - `ssl/cert.pem` - SSL certificate
   - `ssl/key.pem` - SSL private key

2. Uncomment the HTTPS server block in `config/nginx.conf`

3. Comment out the HTTP server block or enable HTTP to HTTPS redirect

4. Update the environment variables in `docker-compose.yml` to use `https://` URLs

5. Restart the services:
   ```bash
   docker-compose restart nginx
   ```

## Network Configuration

- **frontend**: Network for web-facing services (nginx, UIs, API gateway)
- **storage**: Network for backend services (database, elasticsearch, redis)

Services are isolated by network - only nginx has external ports exposed.

## Logs

- Container logs: `docker-compose logs <service_name>`
- Nginx logs: `./logs/nginx` (bind-mounted from `/var/log/nginx`)
- Gateway logs: `./logs/apim-gateway` (bind-mounted from `/opt/graviteeio-gateway/logs`)
- Management API logs: `./logs/apim-management-api` (bind-mounted from `/opt/graviteeio-management-api/logs`)

If log folders are not writable on another machine chmod 777 them.

## Troubleshooting

1. Check all services are running: `docker-compose ps`
2. Check service logs: `docker-compose logs <service_name>`
3. Verify nginx configuration: `docker-compose exec nginx nginx -t`
4. Check network connectivity: `docker-compose exec <service> ping <other_service>`

## Customization

- Modify `config/nginx.conf` to customize nginx behavior
- Update `docker-compose.yml` to change service configurations
- Add custom SSL certificates in the `ssl/` directory
