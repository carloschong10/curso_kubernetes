FROM openjdk:17.0.2

#carpeta de nuestro contenedor
WORKDIR /app

#copiando nuestro jar a nuestra carpeta del contenedor, con el . le indicamos la ruta que definimos en WORKDIR o tambien podriamos definir la ruta /app
COPY ./target/msvc-usuarios-0.0.1-SNAPSHOT.jar .

#exponemos el puerto de nuestro microservicio para poder acceder a través de este a nuestro contenedor
EXPOSE 8001

#lenvatamos el jar que copiamos en app, y no es necesario definir la ruta raiz /app es nuesto directorio de trabajo (WORKDIR)
ENTRYPOINT ["java", "-jar", "msvc-usuarios-0.0.1-SNAPSHOT.jar"]