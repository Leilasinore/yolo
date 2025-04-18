# Yolo Project Docker Implementation Explanation
### Frontend Container
- **Build Stage**: `node:22-slim`
  - It has a minimal size and includes all necessary Node.js tools for building the React application
  - The "slim" variant reduces image size by excluding unnecessary packages
- **Production Stage**: `alpine:3.21.3`
  - Ultra-lightweight Linux distribution, approximately 5MB
  - Significantly reduces final image size
  - Node.js is added only as needed via `apk add npm`

### Backend Container
- **Build Stage**: `node:22`
  - Supports full Node.js development environment and has all necessary build tools
  - Used full version used  and not slim to ensure all potential build dependencies are available
- **Production Stage**: `alpine:3.21.3`
  - Same lightweight base as frontend for consistency
  - Added only essential Node.js runtime is via `apk add --update nodejs`

### Database Container
- **Base Image**: `mongo` (official MongoDB image)
  - Trusted and a well-maintained image from official MongoDB maintainers
  - Pre-configured with optimal MongoDB settings
  - No need for multi-stage build as this is already an optimized image

### Multi-stage Builds
Both frontend and backend employ multi-stage builds to minimize the final image size:
- First stage labeled `AS build` handles dependency installation and building
- Second stage is useful to create a minimal production environment

### Key Directives:
- `WORKDIR /app`: Establishes dedicated application directory to organize files
- `COPY package*.json ./`: Optimizes Docker build cache by copying dependency files first
- `COPY --from=build`: Transfers only necessary artifacts from build stage
- `EXPOSE`: Documents which ports are intended for network connections (3000 for frontend, 4000 for backend)
- `CMD`: Specifies the command to run when container starts
- `ENV NODE_OPTIONS=--openssl-legacy-provider`: Used in frontend to handle legacy dependencies

### Port Allocation
- **Frontend**: Maps container port 3000 to host port 3000
- **Backend**: Maps container port 4000 to host port 4000
- **Database**: Maps container port 27017 to host port 27017

### Bridge Network Implementation
- Created a custom bridge network `app-yolo-net` for container communication
- Bridge driver allows containers to communicate while maintaining isolation from other networks
- Network is made `attachable: true` to allow external containers to connect when needed
- Specified subnet configuration (172.20.0.0/16) for predictable IP addressing
- IP range configuration ensures containers receive IPs from the specified range

### MongoDB Data Persistence
- Created a named volume `app-yolo-mongo-data` for database data persistence
- Volume mounted to MongoDB's standard data directory `/data/db`
- Using `driver: local` for standard local storage on the host

### Volume Benefits:
- Data persists across container restarts and removals

### Naming Convention Used:
- Format: `[username]/[service-name]:[semantic-version]`
  - Example: `leilasinore/leila-yolo-frontend:v1.0.1`

- Username prefix (`leilasinore/`) clearly identifies the image owner
- Service name (`leila-yolo-frontend`) describes the container purpose
- Semantic versioning (`v1.0.1`) indicates compatibility and update significance
- Consistent across all services for easy identification

### Container Naming:
- Used Descriptive container names like `leila-yolo-frontend` instead of auto-generated names
- Maintained Consistent naming pattern across all  containers
- Network and volume names follow the same pattern for consistency

### DockerHub Deployment Screenshot
![Leila Sinore Yolo](/assets/screenshots/leilasinore-yolo.png)