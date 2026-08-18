# ==========================================
# Production Runtime Stage
# ==========================================
FROM node:20-alpine

# Set deployment environment flag
ENV NODE_ENV=production

# Set application directory
WORKDIR /usr/src/app

# 1. Leverage Docker layer caching: copy lockfiles first
COPY package*.json ./

# 2. Install production-only dependencies and purge npm cache to keep layers lean
RUN npm ci --only=production && npm cache clean --force

# 3. Copy application source code
COPY . .

# 4. Security compliance: Change file ownership and drop root privileges
# Uses the built-in 'node' user provided by node:alpine
RUN chown -R node:node /usr/src/app
USER node

# 5. Expose the Express API port
EXPOSE 5000

# 6. Container Health Check to monitor process responsiveness
HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:5000/ || exit 1

# 7. Start the Express API process directly
CMD ["npm", "start"]