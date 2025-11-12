# -----------------------------------------------------------------
# ETAPA 1: El Entorno Base (Node.js + Dependencias de Flutter)
# -----------------------------------------------------------------
FROM node:18-slim

RUN apt-get update && apt-get install -y \
    git \
    curl \
    unzip \
    xz-utils \
    libglu1-mesa \
    && rm -rf /var/lib/apt/lists/*

# -----------------------------------------------------------------
# ETAPA 2: Instalar el SDK de Flutter (Versión Corregida 3.0)
# -----------------------------------------------------------------
WORKDIR /usr/local

RUN curl -o flutter.tar.xz "https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_3.22.2-stable.tar.xz"
RUN tar -xf flutter.tar.xz
RUN rm flutter.tar.xz

# Añadir Flutter al PATH del sistema
ENV PATH="$PATH:/usr/local/flutter/bin"

# CORRECCIÓN 1: Arreglar "flutter: not found"
RUN ln -s /usr/local/flutter/bin/flutter /usr/bin/flutter

# CORRECCIÓN 2: Arreglar "fatal: detected dubious ownership"
RUN git config --global --add safe.directory /usr/local/flutter

# --- ¡LA CORRECCIÓN CLAVE! ---
# CORRECCIÓN 3: Arreglar "SDK version 0.0.0-unknown"
# Forzamos a Flutter a que se configure, descargue binarios y conozca su propia versión.
RUN flutter upgrade
RUN flutter doctor

# -----------------------------------------------------------------
# ETAPA 3: Preparar NUESTRA Aplicación de Node.js
# -----------------------------------------------------------------
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 10000

CMD [ "npm", "start" ]
