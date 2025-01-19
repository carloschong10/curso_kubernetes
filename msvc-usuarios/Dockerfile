FROM openjdk:17.0.2

#carpeta de nuestro contenedor
WORKDIR /app/msvc-usuarios

#copia el pom-xml de curso-kubernetes ejecutando el docker build desde curso-kubernetes
COPY ./pom.xml /app
#copia todos los archivos de msvc-usuarios a la carpeta del directorio de trabajo (WORKDIR)
COPY ./msvc-usuarios .
#como ya estamos dentro del WORKDIR, ahora compilamos con maven
RUN ./mvnw clean install -DskipTests

#exponemos el puerto de nuestro microservicio para poder acceder a través de este a nuestro contenedor
EXPOSE 8001

#lenvatamos el jar que copiamos en el WORKDIR, y en este caso es necesario definir la ruta donde está el jar
ENTRYPOINT ["java", "-jar", "./target/msvc-usuarios-0.0.1-SNAPSHOT.jar"]