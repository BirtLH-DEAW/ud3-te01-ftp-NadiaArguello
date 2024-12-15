# Usa la última versión de Ubuntu como imagen base
FROM ubuntu:latest

# Actualiza la lista de paquetes e instala las actualizaciones disponibles
RUN apt-get update && apt-get upgrade -y

# Instala openssl, proftpd y nano
RUN apt-get install openssl -y
RUN apt-get install proftpd -y
RUN apt-get install nano

#Instala mod-crypto, que contiene mod_tls
RUN apt-get install proftpd-mod-crypto -y

# Crea la carpeta /srv/deaw
RUN mkdir /srv/deaw
RUN mkdir /etc/proftpd/tls

# Copia los ficheros desde la máquina local al contenedor
COPY Recursos_Contenedor_2 /srv/deaw/Recursos_Contenedor_2

# Crea el usuario deaw y su contraseña
  # Crea el usuario, le asigna /srv/deaw como carpeta de trabajo y define la shell del usuario como nologin, impidiendo que el usuario pueda logearse en el sistema
    RUN useradd -d /srv/deaw -s /usr/sbin/nologin deaw 
  # Establece la contraseña deaw para el usuario deaw
    RUN echo "deaw:deaw" | chpasswd
  #Especifica que el usuario y el grupo propietario de /srv/deaw sea deaw
    RUN chown deaw:deaw /srv/deaw
    RUN chown deaw:deaw /srv/deaw/Recursos_Contenedor_2

# Copia los ficheros desde la máquina local al contenedor
COPY Recursos_Contenedor_2 /srv/deaw/Recursos_Contenedor_2

# Sustituye el fichero proftpd.conf previamente configurado de la carpeta conf al contenedor
COPY conf/proftpd.conf /etc/proftpd/proftpd.conf
COPY conf/tls.conf /etc/proftpd/tls.conf

#Descomenta LoadModule mod_tls.c en modules.conf
RUN sed -i 's/#LoadModule mod_tls.c/LoadModule mod_tls.c/' /etc/proftpd/modules.conf

# Copia el certificado autofirmado y la llave al contenedor
COPY conf/certs/cert.pem /etc/proftpd/tls
COPY conf/certs/key.pem /etc/proftpd/tls

# Expone los puertos necesarios
EXPOSE 21 50000-50030

# Iniciar ProFTPD en modo no daemon
CMD ["proftpd", "--nodaemon"]