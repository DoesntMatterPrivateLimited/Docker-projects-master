# Use eclipse-temurin:17 as the base image
FROM eclipse-temurin:17

#Copy all the files from the current dir into the /usr/src/app dir inside the container 
COPY . /usr/src/app/

#Set the working directory inside the container 
#All following commands will run from this directory 
WORKDIR /usr/src/app/

#Complie the java source code
RUN javac Hello.java

#run the compiled java application when the container starts 
CMD ["java", "Hello"]
