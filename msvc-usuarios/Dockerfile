#definimos la variable global ya que la utilizaremos en varios stage (FROM)
#definimos un argumento o variable para reutilizarlo
ARG MSVC_NAME=msvc-usuarios

FROM openjdk:17-jdk-alpine as builder

#declaramos el argumento para que haga referencia al argumento global
ARG MSVC_NAME

#carpeta de nuestro contenedor
WORKDIR /app/$MSVC_NAME

#copia el pom-xml de curso-kubernetes ejecutando el docker build desde curso-kubernetes
COPY ./pom.xml /app
#copia la carpeta .mvn de msvc-usuarios a una carpeta .mvn que creará en el directorio de trabajo (WORKDIR)
COPY ./$MSVC_NAME/.mvn ./.mvn
#copia el batch .mvnw de msvc-usuarios a la carpeta del directorio de trabajo (WORKDIR)
COPY ./$MSVC_NAME/mvnw .
#copia el pom-xml de msvc-usuarios a la carpeta del directorio de trabajo (WORKDIR)
COPY ./$MSVC_NAME/pom.xml .

#como ya estamos dentro del WORKDIR, ahora le decimos a maven no compile ni ejecute el test, y que tambien no compile ni ejecute el main, y que solamente empaquete el proyecto sin los test y sin el codigo fuente y eliminamos la carpeta target
#con esto hacemos que descargue todas las dependecias del proyecto y que queden en CACHE, para que cuando cambiemos algo en el codigo fuente y volvamos a reconstruir la imagen no las vuelva a descargar ya que están en CACHE
RUN ./mvnw clean install -Dmaven.test.skip -Dmaven.main.skip -Dspring-boot.repackage.skip && rm -r ./target/
#dependency:go-offline es similar a la instruccion anterior, solo que descarga unas cositas mas, pero siempre es mejor utilizar la instruccion anterior
#RUN ./mvnw dependency:go-offline

#ahora copiamos el contenido de src de msvc-usuarios a una carpeta src que creará en  directorio de trabajo (WORKDIR)
COPY ./$MSVC_NAME/src ./src

#como ya estamos dentro del WORKDIR, ahora compilamos con maven
RUN ./mvnw clean install -DskipTests


#con esto hacemos que esta segunda imagen sea la que va a producir como imagen final, pero utilizando como base la imagen anterior builder asi hacemos que sea mas liviana ya que no descargará ni utilizará ninguna dependencia
FROM openjdk:17-jdk-alpine
#declaramos el argumento para que haga referencia al argumento global
ARG MSVC_NAME
#definimos nuesto directorio de trabajo de la nueva imagen
WORKDIR /app
#creamos la ruta donde se guardarán los logs
RUN mkdir ./logs
#declaramos otro argumento que reutilice al argumento global MSVC_NAME
ARG TARGET_FOLDER=/app/$MSVC_NAME/target
#y ahora copiamos el jar de la anterior construccion de la imagen a nuestro WORKDIR
COPY --from=builder $TARGET_FOLDER/msvc-usuarios-0.0.1-SNAPSHOT.jar .

#declaramos un argumento para el puerto
ARG PORT_APP=8001
ENV PORT $PORT_APP

#exponemos el puerto de nuestro microservicio para poder acceder a través de este a nuestro contenedor
EXPOSE $PORT

#lenvatamos el jar que copiamos en el WORKDIR, y ya no es necesario definir la ruta ya que nos encontramos ahí al hacer la instruccion COPY
#ENTRYPOINT ["java", "-jar", "msvc-usuarios-0.0.1-SNAPSHOT.jar"]
#con CMD nos permite sobreescribir el comando de entrada por defecto (java -jar msvc-usuarios-0.0.1-SNAPSHOT.jar), mientras que el ENTRYPOINT es mas estricto y es fijo (inmutable)
#eso es importante cuando queramos levantar el contendor en modo interactivo (-it) por ejm: docker run -p 8081:8081 --rm -it msvc-usuarios /bin/sh
CMD ["java", "-jar", "msvc-usuarios-0.0.1-SNAPSHOT.jar"]