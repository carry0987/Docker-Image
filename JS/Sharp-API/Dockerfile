# Build stage
FROM node:21-alpine AS builder

# Set the working directory
WORKDIR /usr/src/app

# Copy the code from the GitHub Actions "JS/Sharp-API/source" directory
COPY source ./

# Install dependencies and build
RUN npm ci
RUN npm run build

# Clean up
RUN npm prune --production && npm cache clean --force

# Production stage
FROM --platform=$TARGETPLATFORM node:21-alpine

# Set the working directory
WORKDIR /usr/src/app

# Copy built node modules and compiled code
COPY --from=builder /usr/src/app/node_modules ./node_modules
COPY --from=builder /usr/src/app/dist ./dist
COPY --from=builder /usr/src/app/package.json ./package.json

# Set the entry point
CMD ["npm", "run", "start"]
