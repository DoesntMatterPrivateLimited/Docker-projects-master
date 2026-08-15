# Use a lightweight version instead of "latest"
FROM nginx:1.23.0-alpine

# default nginx welcome page 
RUN rm -rf  /usr/share/nginx/html/*

# Copy your website files into the nginx html directory
COPY . /usr/share/nginx/html/

# Default port for nginx 
EXPOSE 80

# Default commond 
CMD ["nginx", "-g", "daemon off;"]


