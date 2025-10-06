# Build stage
FROM node:18-alpine AS build

# Set working directory
WORKDIR /app

# Copy package files first for better caching
COPY package.json package-lock.json ./

# Clean npm cache and install with retry logic
RUN npm cache clean --force && \
    npm ci --no-progress --prefer-offline || \
    npm ci --no-progress --prefer-offline || \
    npm install --no-progress --prefer-offline

# Copy source code
COPY . .

# Build the application
RUN npm run build --prod

# Production stage
FROM nginx:alpine

# Copy built application
COPY --from=build /app/dist/frontend /usr/share/nginx/html

# Copy nginx configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80