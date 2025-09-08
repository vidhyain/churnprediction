# syntax=docker/dockerfile:1
FROM mambaorg/micromamba:1.5.8

# Let RUN/CMD use the env without manual "conda activate"
ENV MAMBA_DOCKERFILE_ACTIVATE=1

# Create a conda env named "churn" with Python 3.10 (matches your local venv)
RUN micromamba create -y -n churn -c conda-forge python=3.10 && micromamba clean --all --yes

# App lives here
WORKDIR /app

# Install runtime deps into the conda env from your requirements.txt
# (This should include flask and waitress at minimum.)
COPY requirements.txt .
RUN micromamba run -n churn python -m pip install --no-cache-dir -r requirements.txt

# Copy your code and artifacts (model files, etc.)
# If you know exact files, list them; otherwise copy all.
COPY . .


# Service port
EXPOSE 9696


# Run waitress INSIDE the conda env
# If your app object or module name differ, adjust "predict:app".
CMD ["micromamba","run","-n","churn","waitress-serve","--listen=0.0.0.0:9696","predict:app"] 
 