# Use the official Tomcat image as the base image
FROM tomcat

# Copy the WAR file into Tomcat's webapps directory
# Tomcat automatically deploys WAR files placed here
COPY sample.war /usr/local/tomcat/webapps/

# Document that the application listens on port 8080
EXPOSE 8080

# Start the Tomcat server when the container starts
CMD ["catalina.sh", "run"]
