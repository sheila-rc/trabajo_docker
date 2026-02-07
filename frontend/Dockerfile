# Usamos la imagen oficial de nginx
FROM nginx:alpine

# Copiamos todos los archivos del frontend
# (html, css, js) a la carpeta que nginx sirve
COPY . /usr/share/nginx/html

# Puerto por el que responde nginx
EXPOSE 80
