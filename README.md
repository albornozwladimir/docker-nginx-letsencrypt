# Docker + Nginx + Let's Encrypt
Contenedores Docker para obtener certificados ssl de Let's ENcryp auto renovables con Let's Encrypt en nuestro sistema!

## Docker Compose
Procedemos a crear nuestro directorio  y crear/alojar nuestro docker-compose.yml
```bash
mkdir nombre_proyecto
cd nombre_proyecto
touch docker-compose.yml
```
Luego de esto procedemos a ejecutar nuestro docker-compose.yml con la siguiente instrucción
```bash
docker-compose up -d #Levantamos el contenido del docker-compose.yml y le decimos que se mantenga ejecutandose
```
Lo primero que se realizará será verificar si las imagenes(y su versión) que mencionamos en nuestro **.yml** ya están alojadas en nuestro sistema. De no estarlo, automaticamente se descargarán. 
Para ver las imagenes descargadas y su versión, podemos ejecutar la siguiente instrucción
```bash
docker image ls
```
Luego de lo mencionado en el apartado anterior, Docker generará y desplegará 3 contenedores.
1. El primer contenedor tendrá nuestro nginx-proxy y se encargará de lo siguiente:
   - Comunicarse con la red pública/externa, evitando que el contenedor donde se aloja nuestro sistema haga esta tarea. (Exponiendo el p80 y el p443)
   - Comunicarse con el contenedor Let's Encrypt
   
2. El segundo contenedor tendrá Let'sEncrypt y se encargará de lo siguiente:
   - Conectarse a Let's Encrypt y con las variables de entorno saber para que dominio debe crear un certificado SSL.
   - Por defecto cada 1 hora preguntará si el certificado SSL es válido, de no serlo generará uno nuevo.
   
3. El tercer contenedor tendrá nuestro sitio y se encargará de lo siguiente
   - Exponer a la red interna Docker el puerto 80 (para comunicarse con el contenedor **nginx-proxy**)
   - Desplegar nuestro sitio/sistema
   
Por ultimo, para verificar que nuestros contenedores están deplegados y ejecutandose podemos ejecutar la siguiente instrucción, la cúal nos debería mostrar los 3 contenedores mencionados anteriormente
```bash
docker ps
```

*(Opcional)*
En caso de que se quiera ver el log detallado de todas las acciones que realiza el contenedor de Let's Encrypt para generar el certificado SSL, podemos ejecutar la siguiente instrucción
```bash
docker-compose -f letsencrypt #U otro nombre en caso de que se haya renombrado el contenedor
```

### Herramientas utilizadas / Requisitos

 * ##### Nginx
Herramienta que nos va a permite definir nuestro servidor web y también nuestro proxy. (https://www.nginx.com/)

 * ##### Docker
Herramienta que nos va a permite automatizar el despliegue de nuestro sistema utilizando contenedores de software. (https://www.docker.com/)
Esta herramienta es la única que debe estar instalada en la VM.

 * ##### Docker Compose
 Herramienta que nos va a permitir utilizando un archivo YAML (.yml) simplificar el uso de Docker.

 * #### Let's Encrypt
 Autoridad de certificación gratuita que nos proporcionará certificados gratuitos para el crigrado de seguridad de nuestro sistema. (https://letsencrypt.org/)
 
 * ##### S.O Ubuntu
Corriendo sobre una VM con Ubuntu 18.04 LTS

 * #### Sitio a mostrar
 Debemos tener nuestro Nginx del contenedor "sistema_personal" previamente configurado para que muestre nuestro sistema a través del navegador.
 Luego de esto recomendamos crear una imagen con esta configurtación, cuyo nombre deberá ser ingresado en la opción correspondiente al contenedor "sistema_personal" de nuestro docker-compose.yml
 
 Otra opción es también ocupar una imagen nginx ya creada (https://hub.docker.com/) y simplemente indicarle en el docker-compose.yml donde está el index.html

* #### Dominio y Hosting
