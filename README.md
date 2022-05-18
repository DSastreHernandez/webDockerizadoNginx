# webDockerizadoNginx

Lo primero que hacemos es buscar nginx en DockerHub y ejecutamos la imagen llamada nginx.

![Captura desde 2022-05-18 05-30-19](https://user-images.githubusercontent.com/91556389/169062950-2bf29b8c-3c5a-43e1-8448-f933c38acdcd.png)

Y ejecuta la imagen llamada nginx, lo que la ejecutara y guardara en cache con el siguiente comando(en caso de que no lo tengas descargado te lo descargara):
```
docker run --rm -d -p 8080:80 --name web nginx
```
con el comando anterior, comieza la ejecucion del contenedor como un demonio "-d" desde el puerto 8080 de la red del host. Tambien se aprovecha y se le llama web al contenedor usando la opcion --name.

Y ahora si visitas http://localhost:8080 y veremos que funciona.

![Captura desde 2022-05-18 12-46-05](https://user-images.githubusercontent.com/91556389/169062952-0ab5db65-2cfb-490c-87f7-94eb0b43bf04.png)

Podemos ver el index.html que viene por defecto en nginx. Pero ahora lo personalizaremos, por lo que paramos el contenedor con el siguiente comando:

```
docker stop web
```

## Agregar HTML personalizado

De forma predeterminada, Nginx busca en el directorio /usr/share/nginx/html dentro del contenedor los archivos para servir. Necesitamos poner nuestros archivos html en este directorio.

Dentro del directorio Documents creamos un directorio llamado nginx y dentro de este ultimo otro llamado site-content.

Dentro creamos un fichero index.html que en mi caso contendra:
```
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>html personalizado en docker con nginx</title>
    </head>

    <body>
        <header>
            <h1>Hola, este es el html personalizado.</h1>
            <p>Daniel Sastre Hernandez</p>
        </header>
    </body>
</html>
```

Y un Dockerfile con:

```
FROM nginx:latest
COPY ./site-content/index.html /usr/share/nginx/html/index.html
```

Y con el siguiente comando que es el mismo que el anterior, agragando la marca -v para crear un volumen. que montara el directorio local al directorio del contenedor:

```
docker run --rm -d -p 8080:80 --name web -v ~/Documentos/nginx/site-content:/usr/share/nginx/html nginx
```

Este es el resultado

![Captura desde 2022-05-18 16-45-42](https://user-images.githubusercontent.com/91556389/169069978-143cb305-b81b-411c-bf1d-fa76528e1fd8.png)

