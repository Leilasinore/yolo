# Stage 1: Build the application
FROM node:22 AS build

# Set working directory
WORKDIR /app

# Copy dependency files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the source code
COPY . .

# Stage 2: Create a minimal production image
FROM alpine:3.21.3

WORKDIR /app

RUN apk update && apk add --update nodejs

# Copy built app with dependencies from the build stage
COPY --from=build /app .

# Expose the app's port
EXPOSE 5000

# Start the app
CMD ["node", "server.js"]


