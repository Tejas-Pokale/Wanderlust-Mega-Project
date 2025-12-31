# ------------------- Stage 1: Build Stage ------------------------------
FROM node:21 AS frontend-builder

WORKDIR /app

# Copy dependency manifests first (better caching)
COPY package*.json ./

# Install dependencies (deterministic + faster)
RUN npm ci

# Copy application source
COPY . .

# Build production assets
RUN npm run build


# ------------------- Stage 2: Production Runtime -----------------------
FROM nginx:alpine

# Remove default nginx config
RUN rm /etc/nginx/conf.d/default.conf

# Copy custom nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy built assets from builder stage
COPY --from=frontend-builder /app/dist /usr/share/nginx/html

# Expose HTTP port
EXPOSE 80

# Start nginx
CMD ["nginx", "-g", "daemon off;"]
