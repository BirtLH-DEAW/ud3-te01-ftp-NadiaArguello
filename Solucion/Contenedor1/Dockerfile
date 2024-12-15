# Usa la última versión de Ubuntu como imagen base
FROM ubuntu:latest

# Actualiza la lista de paquetes e instala las actualizaciones disponibles
RUN apt-get update && apt-get upgrade -y

# Instala proftpd y nano
RUN apt-get install proftpd -y
RUN apt-get install nano

# Copia los ficheros desde la máquina local al contenedor
COPY Recursos_Contenedor_1 /srv/ftp/Recursos_Contenedor_1

# Sustituye el fichero proftpd.conf previamente configurado de la carpeta conf al contenedor
COPY conf/proftpd.conf /etc/proftpd/proftpd.conf

# Expone los puertos necesarios
EXPOSE 21 50000-50030