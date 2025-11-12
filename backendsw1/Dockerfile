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
# ETAPA 2: Instalar el SDK de Flutter
# -----------------------------------------------------------------

WORKDIR /usr/local

RUN curl -o flutter.tar.xz "https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_3.22.2-stable.tar.xz"
RUN tar -xf flutter.tar.xz
RUN rm flutter.tar.xz

# 5. Añadir el SDK de Flutter al PATH del sistema
ENV PATH="$PATH:/usr/local/flutter/bin"

# 6. --- ¡LA CORRECCIÓN CLAVE! ---
# Crear un "acceso directo" (symlink) en una carpeta universal (/usr/bin)
# Esto garantiza que Node.js (y 'exec') puedan encontrar "flutter".
RUN ln -s /usr/local/flutter/bin/flutter /usr/bin/flutter

# 7. (Opcional) Aceptar licencias y precargar Flutter
RUN flutter precache
RUN flutter doctor

# -----------------------------------------------------------------
# ETAPA 3: Preparar NUESTRA Aplicación de Node.js
# -----------------------------------------------------------------

WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 10000

# 12. Comando de inicio (Tomado de tu package.json)
CMD [ "npm", "start" ]