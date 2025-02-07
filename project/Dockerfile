# Use Python 3.12.2 image based on Debian Bullseye in its slim variant as the base image
FROM python:3.12.2-slim-bullseye

# Set an environment variable to unbuffer Python output, aiding in logging and debugging
ENV PYTHONBUFFERED=1

# Define a non-root user
ARG USER_ID=1000
ARG GROUP_ID=1000
RUN groupadd -g $USER_ID -o app
RUN useradd -m -u $USER_ID -g $GROUP_ID -o -s /bin/bash app

# Set environment variables for the Django app
ENV DJANGO_SETTINGS_MODULE=project.settings
ENV PORT 8080

# Set the working directory within the container to /app for any subsequent commands
WORKDIR /app

# Copy the entire current directory contents into the container at /app
COPY . /app/

# Upgrade pip to ensure we have the latest version for installing dependencies
RUN pip install --upgrade pip

# Install dependencies from the requirements.txt file to ensure our Python environment is ready
RUN pip install -r requirements.txt

# Change the ownership of the application directory to the non-root user
RUN chown -R app:app /app

# Switch to the non-root user
USER app

# Set the command to run our web service using Gunicorn, binding it to 0.0.0.0 and the PORT environment variable
CMD gunicorn project.wsgi:application --bind 0.0.0.0:"${PORT}"

# Inform Docker that the container listens on the specified network port at runtime
EXPOSE ${PORT}