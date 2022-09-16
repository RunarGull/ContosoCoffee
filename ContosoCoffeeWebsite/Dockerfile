# Use Linux Alpine as a base image with nginx.
FROM nginx:1.23.1-alpine

# Copy ContosoCoffee folder and subdirectories.
ADD /html /usr/share/nginx/html

# Changing the ownership of the files to nginx.
RUN chown -R nginx:nginx /usr/share/nginx/html/

# Allow port 80 to be exposed.
# nginx uses port 80 for http connection.  
EXPOSE 80
