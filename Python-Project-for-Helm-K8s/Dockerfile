# Use Python 3.10 as the base image
FROM python:3.10

# Set the working directory inside the container
WORKDIR /myapp

# Copy all files from the current directory
# into the /myapp directory inside the container
COPY . /myapp

# Install Python dependencies listed in requirements.txt
# --no-cache-dir prevents pip from storing downloaded packages,
# which helps reduce the image size
RUN pip --no-cache-dir install -r requirements.txt

# Run the Python application when the container starts
CMD ["python", "main.py"]
