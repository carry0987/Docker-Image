# Build stage
FROM node:21-alpine AS builder

# Set the working directory
WORKDIR /usr/src/app

# Copy the code from the GitHub Actions "JS/Sharp-API/source" directory
COPY source ./

# Install pnpm
RUN npm install -g pnpm

# Install dependencies and build
RUN pnpm install
RUN pnpm build

# Clean up
RUN pnpm prune --prod && pnpm store prune

# Production stage
FROM --platform=$TARGETPLATFORM node:21-alpine

# Set the working directory
WORKDIR /usr/src/app

# Copy built node modules and compiled code
COPY --from=builder /usr/src/app/node_modules ./node_modules
COPY --from=builder /usr/src/app/dist ./dist
COPY --from=builder /usr/src/app/package.json ./package.json

# Set the entry point
CMD ["node", "dist/src/main"]
