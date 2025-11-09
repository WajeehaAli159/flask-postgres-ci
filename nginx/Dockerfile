FROM nginx:latest

# Remove default nginx config
RUN rm /etc/nginx/conf.d/default.conf

# Copy our custom nginx configuration
COPY sites-enabled/default.conf /etc/nginx/conf.d/default.conf

