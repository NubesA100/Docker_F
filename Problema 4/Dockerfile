# Problema 4
# Cree un dockerfile y comente cada linea a fin de que explique como 
# le hizo para que el contenedor integre un entorno de desarrollo web 
# utilizando nodejs y mysql.

# Comenzando como siempre por unar la imagen oficial de Node.js como base
# En los dockerfiles siempre se empieza por la imagen, en este caso un node:14
FROM node:14

# Instalamos MySQL. Como no especificamos version, se descarga en automatico la ultima version disponible
# y de forma automatica se actualizan las depencias y los paquetes.
# Nuevamente hacemos uso del -y que es en pocas pablabras un "Si a todo"
RUN apt-get update && apt-get install -y default-mysql-server

# Al igual que el ejemplo pasado, necesitamos crear/usar el directorio 
# de trabajo dentro del contenedor
# ¿Por que hacer esto? es importante ya que los comandos
# que se ejecutaran proximamente pueden generar erroes, confligtos, confuciones entre otros 
# problemas. 
WORKDIR /usr/src/app

# Copiar los archivos package.json y package-lock.json para instalar las dependencias, esto
# con la finalidad de evitar problemas a futuro cercano como una inconsistencia en las versiones,
# instalacion incorrecta de las versiones y mayor tiempo de configuraciones manuales. Por eso es
# importante en el Dockerfile 
COPY package*.json ./

# Una vez copiado la lista de dependencias, procedemos a intalar las mismas dependencias 
# definidas en package.json. La explicacion para un niño de 8 años de la relacion entre 
# COPY package*.json ./ y RUN npm install es que el copy es la lista del super y el run es 
# el tiendero que te surte todo y te lo despacha practicamente.
RUN npm install

# Copiar el resto de los archivos de la aplicación al contenedor en donde se copian todos los archivos
# que se ubican en path del dockerfile hacia el workdir del contenedor
COPY . .

# Exponer el puerto que usará la aplicación (por defecto, Node.js utiliza el puerto 3000 segun las normas internacionales)
EXPOSE 3000

# Comando para iniciar la aplicación cuando se ejecute el contenedor y verificar que todo jala bien
CMD ["node", "app.js"]