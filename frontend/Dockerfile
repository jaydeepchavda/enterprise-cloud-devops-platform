# ==========================================
# STAGE 1: Build Environment
# ==========================================
FROM node:20-alpine AS builder

WORKDIR /app

# 1. Cache dependencies: copy lockfiles first
COPY package*.json ./

# 2. Install dependencies needed to compile the application
RUN npm install

# 3. Copy source code
COPY . .

# 4. Compile React app into production static bundle (/app/build or /app/dist)
RUN npm run build

# ==========================================
# STAGE 2: Production Web Server (Nginx)
# ==========================================
FROM nginx:alpine AS runner

# 1. Remove default Nginx welcome site
RUN rm -rf /usr/share/nginx/html/*

# 2. Copy your custom Nginx config for SPA routing (try_files)
COPY nginx.conf /etc/nginx/conf.d/default.conf

# 2. Copy compiled static assets from builder stage
# (Note: Check if your repo builds to 'build' or 'dist' — CRA defaults to 'build')
COPY --from=builder /app/dist /usr/share/nginx/html

# 3. Expose standard HTTP port
EXPOSE 80

# 4. Run Nginx in foreground to keep container active
CMD ["nginx", "-g", "daemon off;"]