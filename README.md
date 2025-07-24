# GitHub Action for Firebolt Core Setup

This GitHub Action sets up Firebolt Core Docker containers with certificates and waits for service readiness. It also automatically handles cleanup when the workflow completes.

## Action: Setup Firebolt Core

Sets up Firebolt Core Docker containers with certificates and waits for service readiness.

**Inputs:**
- `tag_version` (optional, default: 'preview-rc'): The Docker image tag for Firebolt Core

**Outputs:**
- `docker_compose_file`: Path to the Docker Compose file
- `service_url`: HTTP URL of the running service (`http://localhost:3473`)
- `service_https_url`: HTTPS URL of the running service (`https://localhost:443`)

**Example usage:**
```yaml
- name: Setup Firebolt Core
  id: setup-core
  uses: firebolt-db/action-setup-core@main
  with:
    tag_version: ${{ inputs.tag_version }}
```

## What the Action Does

### Setup Process
1. **Repository Checkout**: Checks out the action repository to access resources
2. **Docker Setup**: Logs into GitHub Container Registry and sets up Docker Buildx with caching
3. **Certificate Generation**: Creates self-signed certificates for localhost using `trustme`
4. **Certificate Installation**: Installs certificates to the system keystore
5. **Image Tag Resolution**: Uses the provided `tag_version` input (defaults to 'preview-rc')
6. **Docker Compose Preparation**: Updates the docker-compose.yml with the correct image tag and base directory
7. **Container Startup**: Starts the Firebolt Core and nginx containers
8. **Readiness Check**: Waits for the service to be ready by polling `http://localhost:3473` (max 30 retries, 2-second intervals)

### Automatic Cleanup
The action automatically handles cleanup using `gacts/run-and-post-run@v1`, which ensures containers are stopped and removed when the workflow completes, regardless of success or failure.

## Configuration Details

The action uses hardcoded configuration values:
- **Service URL**: `http://localhost:3473`
- **HTTPS Service URL**: `https://localhost:443`
- **Max Retries**: 30 attempts
- **Retry Interval**: 2 seconds
- **Default Image Tag**: 'preview-rc'
