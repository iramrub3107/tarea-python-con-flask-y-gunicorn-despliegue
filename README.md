# Despliegue de una aplicación utilizando Python con Flask y Gunicorn | Tarea realizada por Izan Ramos Rubio

---

# 1. DESPLIEGUE

Para comenzar con nuestro despliegue, primero que todo necesitaremos instalar varios paquetes:

1. Primero, nos instalaremos el gestor de paquetes de Python (pip) ejecutando ```sudo apt install -y python3-pip```

<img width="1323" height="746" alt="image" src="https://github.com/user-attachments/assets/e63db5ac-f7bc-428f-a76e-7937c77d4164" />

2. A continuación, nos instalaremos pipenv con ```pip3 install pipenv```. Este "paquete" nos permitirá gestionar entornos virtuales.

<img width="1325" height="746" alt="image" src="https://github.com/user-attachments/assets/2bfaba6e-9626-4147-859a-8e66a02f6d55" />

Antes de continuar, nos aseguraremos de que pipenv se ha instalado correctamente ejecutando ```pipenv --version```. Aunque poniendo solamente el comando en sí este funciona a la primera,
en mi caso he tenido que poner justo antes del comando "~/.local/bin/" para que se pudiera ejecutar correctamente. Si no lo hacía, bash interpretaba "pipenv" como un comando a secas, no lo llegaba a encontrar y decía que no encontraba ese comando.

<img width="1323" height="744" alt="image" src="https://github.com/user-attachments/assets/f6804a75-dd7d-412c-9057-586bb8d31683" />

3. Finalmente, nos descargaremos python-dotenv para cargar las variables de entorno ejecutando ```pip3 install python-dotenv```

<img width="994" height="698" alt="image" src="https://github.com/user-attachments/assets/d355f97d-e483-4822-b32e-5250c7d791d4" />

---

Ahora que ya tenemos nuestros paquetes descargados, vamos a comenzar con la creación de la aplicación.
Para esto, vamos a crear la carpeta en donde se situará nuestra aplicación. Haremos que nuestro usuario, además de pertenecer al grupo www:data, también sea el propietario de nuestra nueva carpeta, además de que todo el mundo pueda leer esta nueva carpeta.
Si queremos hacer todo esto, tendremos que ejecutar los siguientes comandos:

1. ```sudo mkdir -p /var/www/app-despliegue-izan```. Yo le he puesto ese nombre a la carpeta, pero puede tener cualquier otro nombre.
2. ```sudo chown -R usuario:www-data /var/www/app-despliegue-izan```. Mi usuario se llama tal cual "usuario", aunque esto se tiene que hacer con el usuario del que se disponga.
3. ```sudo chmod -R 775 /var/www/app-despliegue-izan```

<img width="1323" height="744" alt="image" src="https://github.com/user-attachments/assets/cef1bf5a-6878-4547-a00f-40ac6d726058" />

---

Dentro de nuestra nueva carpeta, crearemos un archivo .env que estará oculto:

<img width="1322" height="745" alt="image" src="https://github.com/user-attachments/assets/337e3fdb-27d6-4653-8c60-e1145f9dcc41" />

Y dentro del archivo .env, pondremos estas dos simples líneas:

<img width="1322" height="746" alt="image" src="https://github.com/user-attachments/assets/924ec436-a6d9-4909-b56e-1a1fd46894cf" />

Ahora ejecutaremos el comando ```pipenv shell``` (si es necesario, poner "~/.local/bin/" justo antes del comando). Si todo sale bien, podremos iniciar el entorno virtual de nuestro proyecto, y nos aparecerá entre paréntesis el nombre de nuestro proyecto al inicio del prompt 

<img width="1323" height="746" alt="image" src="https://github.com/user-attachments/assets/54dcd55e-c90f-4e4f-94f4-7558808edade" />

Y cuando tengamos iniciado nuestro entorno virtual, instalaremos Gunicorn y flask; esenciales para nuestra aplicación.

<img width="1324" height="449" alt="image" src="https://github.com/user-attachments/assets/528ce88f-8fcc-443b-8686-fc5a6288d1b0" />

Como ahora mismo no queremos hacer una aplicación demasiado grande, haremos una mucho más simple en la que crearemos solamente dos archivos: **application.py** y **wsgi.py**

<img width="1322" height="746" alt="image" src="https://github.com/user-attachments/assets/844f0a3e-5f7f-44b2-829b-329caf42ad19" />

A continuación, iremos añadiendo el código necesario para cada uno de los dos archivos. Empezando por application.py, pondremos el código que aparece en la siguiente captura de pantalla:

<img width="1323" height="746" alt="image" src="https://github.com/user-attachments/assets/0982eb02-f23b-48a3-954d-6d8884d1f0cc" />

Y después, pondremos este otro código en wsgi.py:

<img width="1321" height="746" alt="image" src="https://github.com/user-attachments/assets/fd478a30-4284-4ba6-8216-17cb461b8702" />

---

Antes de desplegar la aplicación que acabamos de crear, hay que asegurarse de que se pueda ver desde nuestro navegador. Para ello, iremos a la configuración de la máquina virtual desplegada, y entraremos en Configuración > Red > Reenvío de puertos. Ahí dentro, crearemos una nueva regla de reenvío en la que pondremos esta configuración:

<img width="1323" height="745" alt="image" src="https://github.com/user-attachments/assets/4f8db838-236d-4892-8eca-295f0c250b69" />

Esta configuración nos servirá bastante, ya que así, cuando despleguemos nuestra aplicación, la podremos ver desde el localhost en el puerto 5000, tal y como lo hemos configurado.

Una vez tengamos hecha nuestra configuración, toca desplegar nuestra aplicación para ver si funciona. Para ello, ejecutaremos ```flask run --host '0.0.0.0'``` para desplegarla usando flask, sin olvidarnos de hacerlo teniendo iniciado nuestro entorno virtual:

<img width="1325" height="741" alt="image" src="https://github.com/user-attachments/assets/6b225253-3698-44e1-8906-2e7200e606d7" />

Y como podremos observar, nuestra aplicación se ha desplegado correctamente:

<img width="1323" height="531" alt="image" src="https://github.com/user-attachments/assets/f539b485-4e93-41ae-863b-b82c37831574" />

Ya que flask funciona correctamente, ahora toca hacer lo mismo pero con Gunicorn. Para ello, ejecutaremos ```gunicorn --workers 4 --bind 0.0.0.0:5000 wsgi:app```, haciéndolo teniendo iniciado nuestro entorno virtual:

<img width="1323" height="744" alt="image" src="https://github.com/user-attachments/assets/2a8c26b9-18cd-40ab-bb5d-b26809969d13" />

Ahora comprobaremos si funciona...

<img width="1323" height="589" alt="image" src="https://github.com/user-attachments/assets/10996eff-49db-4559-a8a3-7ed7bcf79bca" />

Y como se puede observar, la aplicación también se despliega correctamente utilizando Gunicorn.

Ahora, vamos a parar la ejecución de la aplicación para realizar algo que nos servirá bastante más adelante. Ejecutaremos ```which gunicorn``` para poder ver la ruta desde donde se ejecuta Gunicorn y la copiaremos en algún lado para poder pegarla más tarde.

<img width="1203" height="678" alt="image" src="https://github.com/user-attachments/assets/c4515c64-ac39-4dfd-8c9b-f51e0e1fd8a0" />

---

A continuación, vamos a iniciar nginx dado a que nos va a ser necesario para desplegar la aplicación más adelante. 
En el caso de que no tengamos instalado el servicio (cosa que sería algo extraña al desplegar una máquina virtual con Vagrant), nos lo instalaremos ejecutando ```sudo apt install nginx```
Después de iniciarlo, vamos a comprobar también su estado:

<img width="1203" height="678" alt="image" src="https://github.com/user-attachments/assets/f3fbc96f-3c58-4379-b9c7-a729934c01ae" />

Una vez tengamos iniciado correctamente nginx, vamos a crear un servicio en el cual tendremos ahí "configurado" nuestro Gunicorn para que nuestro systemd lo detecte como un servicio más.
Este archivo se llamará flask_app.service y se encontrará en la carpeta en donde están todos los demás servicios que tenemos: en /etc/systemd/system
En él pondremos el contenido que aparece en la captura de pantalla:

<img width="1203" height="676" alt="image" src="https://github.com/user-attachments/assets/1815d4da-cac0-48da-a06d-6a276718b7ff" />

Cuando tengamos configurado nuestro nuevo servicio, ejecutamos ```sudo systemctl daemon-reload``` para que se "añada" el nuevo servicio que acabamos de crear:

<img width="1203" height="676" alt="image" src="https://github.com/user-attachments/assets/7ecc1261-f0e1-4d84-9e03-ed4b5757bb11" />

Y, posteriormente, vamos a habilitar e iniciar nuestro servicio

<img width="1203" height="676" alt="image" src="https://github.com/user-attachments/assets/31ce120b-9e22-4ed8-8f41-d8469ff6d3f5" />

Ahora, vamos a crear un fichero en /etc/nginx/sites-available que se llamará app.conf, en donde tendremos ahí metida la configuración que aparece en la captura de pantalla para que nuestro nginx esté bien configurado para que, más adelante, nuestra aplicación pueda funcionar sin problemas

<img width="1203" height="676" alt="image" src="https://github.com/user-attachments/assets/cf31b324-b897-4e92-bf16-fc8ba3ea4850" />

Posteriormente, vamos a crear un enlace simbólico de nuestro archivo recién creado que se encontrará en /etc/nginx/sites-enabled. Para ello, ejecutaremos ```sudo ln -s /etc/nginx/sites-available/app.conf /etc/nginx/sites-enabled/```

<img width="1203" height="676" alt="image" src="https://github.com/user-attachments/assets/67073849-bced-4942-9693-c6dd6c19532b" />

Por si acaso, vamos a asegurarnos de que el enlace simbólico se ha creado correctamente

<img width="1203" height="676" alt="image" src="https://github.com/user-attachments/assets/47d17921-452e-4d76-9a18-1f8d06a2cbf0" />

Como se puede observar en la captura de pantalla, mi enlace se ha creado correctamente.

Ahora, comprobaremos que toda la configuración de nginx se ha realizado bien ejecutando ```sudo nginx -t```, y, en el caso de que todo está OK reiniciamos el servicio de nginx y luego revisaremos su estado. Si por algún casual, nginx detecta alguna configuración mal hecha, te lo advertirá y, por ende, tendrás que arreglarlo antes de reiniciar el servidor, ya que, si no revisas la configuración, pueden surgir problemas a la hora de reiniciar el servicio.

<img width="888" height="626" alt="image" src="https://github.com/user-attachments/assets/7005dad5-d005-4d5e-8688-7b9969f9984f" />

Como se puede observar desde la captura de pantalla, al haber ejecutado ```sudo nginx -t```, nginx me ha dicho básicamente que toda mi configuración está bien y, por ende, he reiniciado el sistema y he mirado su estado.

Para que podamos ver bien la aplicación, vamos a tener que cambiar la configuración de la red de la VM. Para ello, simplemente iremos a la VM e iremos a Configuración > Red y cambiaremos la Red NAT por Adaptador Puente. Este cambio se tiene que realizar ya que, después de configurar el archivo /etc/hosts de la máquina anfitriona, al intentar acceder a la aplicación se "queda cargando en bucle". Es decir, resuelve el nombre de la aplicación (que es app.izv) pero la conexión HTTP nunca terminará de completarse, formando ese "bucle". Además, en la Red NAT no podremos sacarle la IP a la VM por estar en la Red NAT

<img width="1203" height="678" alt="image" src="https://github.com/user-attachments/assets/32ec1075-a567-4bc4-8b79-92cfcb3db398" />

Y ahora, después de reiniciar la VM, podremos observar que al ejecutar ```ip a```, ya podremos ver que esta ya tiene dirección IP.

<img width="1203" height="677" alt="image" src="https://github.com/user-attachments/assets/3b7af4d3-f17d-4dbe-91c5-f844b3b6dfa0" />

Cuando ya hayamos hecho ese cambio, ahora nos iremos a nuestro dispositivo anfitrión y hacemos los cambios necesarios en nuestro archivo "hosts". En mi caso, es en /etc/hosts:

<img width="1203" height="677" alt="image" src="https://github.com/user-attachments/assets/0fb40280-b5ec-4fe5-9242-cd00573cc930" />

Como se puede observar en la captura de pantalla, en ese archivo se ha puesto la IP de la VM más abajo seguido de los dominios correspondientes.

Y con toda las configuraciones que hemos estado realizando, podemos observar que nuestra aplicación funciona correctamente!

<img width="1200" height="515" alt="image" src="https://github.com/user-attachments/assets/63e4778d-7ba3-4adc-b440-24d2473d01b6" />

---

# 2. TAREA DE AMPLIACIÓN

Para empezar con esta tarea de ampliación en la cual tendremos que hacer el mismo proceso que en "Despliegue", pero con un repo de GitHub, vamos a hacer lo siguiente:

Primero que todo, vamos a irnos al directorio /var/www y vamos a clonar (ejecutando git clone <enlace-github>) este repositorio: **https://github.com/Azure-Samples/msdocs-python-flask-webapp-quickstart**, no sin antes tener instalado git en el caso de que no lo tengamos instalado:

<img width="1202" height="674" alt="image" src="https://github.com/user-attachments/assets/4cc02712-def9-4c0c-aa28-ce7b5b4fd840" />

<img width="1203" height="677" alt="image" src="https://github.com/user-attachments/assets/c90a349a-dc20-44e2-aa5c-4b52fe680575" />

También es necesario instalarse los paquetes que nos habíamos instalado al principio del primer despliegue: python3-pip, pipenv, python-dotenv... Pero como ya están instalados de haber hecho la anterior práctica, no es necesario hacerlo de nuevo.

---

A continuación, haremos que nuestro usuario sea propietario de la carpeta del repo que hemos clonado, y también aplicaremos los permisos necesarios en esta carpeta para que todo el mundo lo pueda leer
Para ello, ejecutaremos dos comandos:

```
sudo chown -R usuario:www-data /var/www/msdocs-python-flask-webapp-quickstart/
sudo chmod -R 775 /var/www/msdocs-python-flask-webapp-quickstart/
```
Hay que tener en cuenta que, en donde pone "usuario", hay que cambiarlo para que sirva con el usuario del que se disponga.

<img width="1201" height="676" alt="image" src="https://github.com/user-attachments/assets/4944ff89-7e83-44c4-984d-f5b2ea4b5781" />

Dentro del directorio del nuevo proyecto, creamos el archivo oculto .env y pondremos el contenido que se señala en la captura de pantalla:

<img width="1203" height="675" alt="image" src="https://github.com/user-attachments/assets/4d2cdcca-0f2f-45c8-9b54-5d9233c0ff89" />

Ahora, iniciaremos nuestro entorno virtual ejecutando ```pipenv shell```, y observaremos que se nos ha iniciado (recordar que, para confirmar que se ha iniciado, debe de aparecer entre paréntesis el nombre del directorio antes del prompt)

<img width="1203" height="675" alt="image" src="https://github.com/user-attachments/assets/9e6795b8-f7e7-4649-b728-8dd0f19f1dda" />

---

A continuación, instalaremos las dependencias que el propio proyecto que hemos clonado incluye en el archivo .txt que tiene este. Para poder instalarlas, ejecutaremos ```pipenv install -r requirements.txt```
requirements.txt es el nombre del archivo con las dependencias. En el caso de que el archivo tenga otro nombre, se cambia la parte de "requirements.txt" por la del nombre del archivo con las dependencias.

<img width="1203" height="677" alt="image" src="https://github.com/user-attachments/assets/4fc50c36-1efb-43d1-a99c-ae3e09c5e0d6" />

Después de instalar las propias dependencias del proyecto, pondremos a correr nuestra aplicación iniciando Gunicorn ejecutando ```gunicorn --workers 4 --bind 0.0.0.0:5000 wsgi:app```, y cuando termine de cargar, comprobaremos si todo va bien

<img width="1203" height="677" alt="image" src="https://github.com/user-attachments/assets/ba89cc24-e35b-4f81-9a23-4faf961009c0" />

Una vez iniciado Gunicorn, introduciremos en el navegador la IP de la VM + :5000 (su IP en el puerto 5000) y verificamos si todo va bien

<img width="1202" height="683" alt="image" src="https://github.com/user-attachments/assets/7bb555aa-b20c-4553-ac7b-5eb7248dbf31" />

Como podemos observar en la captura de pantalla, la aplicación funciona correctamente.

---

Ahora haremos el proceso de crear un servicio para que Gunicorn sea uno de estos. Para ello, primero sacaremos la ruta real de Gunicorn ejecutando ```which gunicorn```. Cuando la tengamos, nos saldremos el entorno virtual

<img width="1202" height="678" alt="image" src="https://github.com/user-attachments/assets/6b7818e7-961f-4470-8054-4a8bf3735c55" />

Después, adaptaremos el archivo /etc/systemd/system/flask_app.service que previamente creamos para la aplicación de prueba anterior, para que nginx funcione correctamente con la aplicación en la que estamos trabajando ahora.
Primero, entramos en la carpeta donde está el archivo, y lo editamos con ```sudo nano flask_app.service```

<img width="1203" height="677" alt="image" src="https://github.com/user-attachments/assets/77ebe29f-478e-4bb9-8422-df32b7d88ef1" />

Dentro del archivo, pondremos el código que aparece en esta captura de pantalla:
<img width="1200" height="675" alt="image" src="https://github.com/user-attachments/assets/036f3bd1-a888-4139-ae37-269db7b5e28e" />

Después, recargamos todos los servicios (además de los cambios en el flask_app.service) ejecutando ```sudo systemctl daemon-reload```, y habilitaremos e iniciaremos nuestro servicio adaptado. También comprobaremos su estado por si ha fallado algo al iniciarlo:

<img width="1203" height="676" alt="image" src="https://github.com/user-attachments/assets/23dd6ab3-ae43-4062-a467-b1af503679ea" />

---

Y ahora, vamos a adaptar nuestro archivo app.conf situado en /etc/nginx/sites-available y cambiaremos lo necesario ahí dentro para que nginx funcione con nuestra nueva aplicación:

<img width="1203" height="676" alt="image" src="https://github.com/user-attachments/assets/cbbffd22-7b82-4a29-90eb-9694e0b94bb2" />

Y haremos el enlace simbólico del archivo recién editado y lo pondremos en /etc/nginx/sites-enabled ejecutando ```sudo ln -s /etc/nginx/sites-available/app.conf /etc/nginx/sites-enabled/```

<img width="961" height="716" alt="image" src="https://github.com/user-attachments/assets/7b38e57a-3d0b-4d5e-955c-d0922552c135" />

Además, comprobaremos la sintaxis del fichero ejecutando ```sudo nginx -t``` y reiniciamos el servicio

<img width="961" height="275" alt="image" src="https://github.com/user-attachments/assets/26a3ff63-c333-4f33-8468-8a0ba0924068" />

Ahora nos iremos a nuestro dispositivo anfitrión y hacemos los cambios necesarios en nuestro archivo "hosts". En mi caso, es en /etc/hosts. Pondremos la IP de la VM y los dominios que corresponden

<img width="962" height="541" alt="image" src="https://github.com/user-attachments/assets/e0338cce-e151-459a-ba94-6a0411891c60" />

Y finalmente, entraremos en app.izv o www.app.izv, y comprobamos si nuestra aplicación se ha desplegado correctamente:

<img width="962" height="541" alt="image" src="https://github.com/user-attachments/assets/57d9e955-06f7-4200-afa3-885d406af992" />

Y podremos observar en la captura de pantalla que la aplicación ha sido desplegada con éxito
