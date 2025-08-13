# Dockerfile
# Use an official Python runtime as a parent image. This provides a base operating system
# with Python pre-installed, making the image lightweight and efficient.
FROM python:3.11-slim-buster
# Using Python 3.11 for compatibility.

# Set the working directory inside the container to /app.
# All subsequent commands (like COPY, RUN, CMD) will be executed relative to this directory.
WORKDIR /app

# Copy the requirements.txt file from your host machine into the container at /app.
# This step is done early to leverage Docker's build cache.
COPY requirements.txt .

# Install any needed Python packages specified in requirements.txt.
# --no-cache-dir: Prevents pip from storing cache, reducing the final image size.
# -r: Installs packages listed in the requirements file.
RUN pip install --no-cache-dir -r requirements.txt

# Copy the main FastAPI application file (e.g., app.py or app_flexible.py) into the container at /app.
COPY app.py /app/

# Copy the 'src' directory (if it exists) containing config.py and other custom modules.
# This is crucial for resolving Python imports like 'from src import config'.
COPY src/ /app/src/

# Copy the 'model_store' directory containing your trained model artifacts.
# Ensure that 'model_store/churn_prediction_model_v1.joblib' (or your actual model file)
# exists in your host machine's project directory before building the Docker image.
COPY model_store/ /app/model_store/

# Expose port 8000. This informs Docker that the container listens on the specified network port
# at runtime. It doesn't actually publish the port; it's documentation and can be used by other tools.
EXPOSE 8000

# Command to run the application using Uvicorn when the container starts.
# 'app:app' refers to the 'app' object inside 'app.py' (or 'app_flexible.py').
# '--host 0.0.0.0' makes the server accessible from outside the container (from any IP address).
# '--port 8000' specifies the port Uvicorn listens on inside the container.
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]