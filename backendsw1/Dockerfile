# -----------------------------------------------------------------
# ETAPA 1: El Entorno Base (Node.js + Dependencias de Flutter)
# -----------------------------------------------------------------
# Usamos Node 18, que es compatible con la mayoría de los builds
FROM node:18-slim

# Instalar dependencias del sistema (git, curl, etc.)
RUN apt-get update && apt-get install -y \
    git \
    curl \
    unzip \
    xz-utils \
    libglu1-mesa \
    && rm -rf /var/lib/apt/lists/*

# -----------------------------------------------------------------
# ETAPA 2: Instalar el SDK de Flutter (Versión Corregida)
# -----------------------------------------------------------------
WORKDIR /usr/local

# Descargar y descomprimir Flutter
RUN curl -o flutter.tar.xz "https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_3.22.2-stable.tar.xz"
RUN tar -xf flutter.tar.xz
RUN rm flutter.tar.xz

# Añadir Flutter al PATH del sistema
ENV PATH="$PATH:/usr/local/flutter/bin"

# --- INICIO DE LAS CORRECCIONES ---

# CORRECCIÓN 1: Arreglar el error "flutter: not found" de Node.js
# Creamos un "acceso directo" (symlink) en una carpeta universal.
RUN ln -s /usr/local/flutter/bin/flutter /usr/bin/flutter

# CORRECCIÓN 2: Arreglar el error "fatal: detected dubious ownership"
# Le decimos a Git que es seguro correrlo como root en este directorio.
RUN git config --global --add safe.directory /usr/local/flutter

# CORRECCIÓN 3: Arreglar el error "SDK version 0.0.0-unknown"
# Forzamos a Flutter a descargar todos sus binarios y auto-revisarse.
# Esto se ejecuta DESPUÉS de arreglar los permisos de Git.
RUN flutter doctor
RUN flutter precache

# --- FIN DE LAS CORRECCIONES ---

# -----------------------------------------------------------------
# ETAPA 3: Preparar NUESTRA Aplicación de Node.js
# -----------------------------------------------------------------
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 10000

# Comando de inicio (de tu package.json)
CMD [ "npm", "start" ]
