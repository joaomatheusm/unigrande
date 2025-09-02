# Pull official base image for Python 3.11.2 and Debian 12 (bookworm)
FROM python:3.11.2-slim

# Cria um usuário não-root (se já não existir)
RUN useradd -m mth

# Set working directory
WORKDIR /usr/src/app

# Instala dependências do sistema enquanto é root
USER root

RUN apt-get update && apt-get install -y netcat-openbsd && apt-get clean && rm -rf /var/lib/apt/lists/*

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# Install Python dependencies
RUN pip install --upgrade pip

# Copy and install python dependencies from requirements.txt
COPY ./requirements-dev.txt .
RUN pip install --no-cache-dir -r requirements-dev.txt

COPY ./requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Add entrypoint.sh
COPY ./entrypoint.sh .
RUN chmod +x /usr/src/app/entrypoint.sh

# Altera o dono do diretório para o novo usuário
RUN chown -R mth:mth /usr/src/app

# Define o usuário no Dockerfile para a execução da aplicação
USER mth

# Run entrypoint.sh
ENTRYPOINT ["/usr/src/app/entrypoint.sh"]
