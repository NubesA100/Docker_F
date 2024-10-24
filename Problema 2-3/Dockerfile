# Explique como un contenedor puede integrar un entorno de desarrollo utilizando 
# un web server y una base de datos dentro de un mismo contenedor.
# ¿Como lograr este objetivo?
# De manera teorica debemos de crear un contenedor de ubuntu que albergue el servidor web y el SQL
# En este Dockerfile albergaremos una imagen de ubuntu, concretamente la 
# version 20.04 la cual es la version reciente y estable. En base a esta imagen
# crearemos un contenedor de ubuntu
FROM ubuntu:20.04  

# Establecer el directorio de trabajo, lo cual es importante debido a que los comandos
# que vienen a continuacion pueden generar erroes, confligtos, confuciones entre otros 
# problemas. Este contenedor no tiene persitencia de datos debido a que no le creamos un 
# volumen para que pueda almacenar los datos
WORKDIR /var/www/html

# Instalamos las actualizaciones de ubuntu y dependencias del servidor web y el sql
# en donde encontramos que:
# apt-get update sirve para actualizar los paquetes de ubuntu 
# apt-get install -y \ (dependencias de sql, apache y php)  sirve para descargar los elementos requeridos.
# agregar el que "-y" significa que daremos si a todo lo que nos pregunte
# && apt-get clean // este ultimo sirve para limpiar el cache generado por estas descargas
RUN apt-get update && apt-get install -y \
    apache2 \
    mysql-server \
    php \
    libapache2-mod-php \
    php-mysql \
    && apt-get clean

# Este comando sirve para copiar archivos de la aplicación(de mi maquina real) al servidor web que
# se encuentra dentro del contenedor. Si no se añade este fragmento, habria que añadirse los recursos 
# necesarios manualmente y de forma repetitiva cada que querramos contruir un nuevo contenedor
COPY ./mi_aplicacion /var/www/html/

# Aqui arrancamos MySQL y le agregamos una contraseña. Posteriormente creamos una simple base de datos
# y la nombramos mydatabase
RUN service mysql start && \
    mysql -e "ALTER USER 'root'@'localhost' IDENTIFIED BY 'secret';" && \
    mysql -e "CREATE DATABASE mydatabase;"

# Configurar las variables de entorno para MySQL, el cual pide al momento de instalar las dependencias del paso 2
# Encontramos que estas variables corresponden a lo siguiente:
# MYSQL_REGION=2 para configurar el continente, en este caso, el 2 se refiere al contiente americano
# MYSQL_CITY=98 esta variable de entorno, identifica la ciudad, para cdmx el valor es 98 y para monterrey el
# valor es 102
# MYSQL_TIMEZONE=98 al igual que el anterior, esta variable sirve para identificar la zona horaria. Se usan los
# mismos digitos que en la anterior ya que comparten la numeracion (cdmx=98)
ENV MYSQL_REGION=2
ENV MYSQL_CITY=98
ENV MYSQL_TIMEZONE=98

# Exponemos los puertos para las comunicaciones
EXPOSE 80 3306

# Iniciamos los servicios y dejamos corriendo en segundo plano
CMD service mysql start && apache2ctl -D FOREGROUND