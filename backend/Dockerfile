# Imagen base
FROM node:18-alpine

# Carpeta de trabajo dentro del contenedor
WORKDIR /app

# Copio primero los package para instalar dependencias
COPY package*.json ./

RUN npm install

# Copio el resto del código
COPY . .

# Puerto de la API
EXPOSE 3000

# Comando para arrancar el servidor
CMD ["npm", "start"]
