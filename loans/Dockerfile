# Use Amazon Linux 2023
FROM amazonlinux:2023

# Install NGINX
RUN dnf install -y nginx && dnf clean all

# Create website directory
RUN mkdir -p /usr/share/nginx/html

# Copy your website files into Nginx default directory
COPY . /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start NGINX in the foreground
CMD ["/usr/sbin/nginx", "-g", "daemon off;"]
