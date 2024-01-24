# Stage 1: Build stage
FROM node:20-alpine AS builder
WORKDIR /usr/src/app

# Copy the code from the GitHub Actions "JS/Sharp-API/source" directory
COPY source ./

# Install dependencies and build
RUN npm ci --only=production && npm cache clean --force
RUN npm run build

# Production stage
FROM --platform=$TARGETPLATFORM node:20-alpine
WORKDIR /usr/src/app

# Copy built node modules and compiled code
COPY --from=builder /usr/src/app/node_modules ./node_modules
COPY --from=builder /usr/src/app/dist ./dist

CMD ["node", "dist/server.js"]
