# Use an official Python runtime as a parent image
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container
COPY . .

# Install any needed dependencies specified in requirements.txt
RUN pip install --no-cache-dir flask

# Expose port 5000 to the host
EXPOSE 5000

# Run the application when the container launches
CMD ["python", "app.py"]
