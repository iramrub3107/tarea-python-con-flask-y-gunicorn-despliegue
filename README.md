# Despliegue de una aplicación utilizando Python con Flask y Gunicorn
# Tarea realizada por Izan Ramos Rubio

---

# 1. Despliegue

Para comenzar con nuestro despliegue, primero que todo necesitaremos instalar varios paquetes:

1. Primero, nos instalaremos el gestor de paquetes de Python (pip) ejecutando ```sudo apt install -y python3-pip```

<img width="1323" height="746" alt="image" src="https://github.com/user-attachments/assets/e63db5ac-f7bc-428f-a76e-7937c77d4164" />

2. A continuación, nos instalaremos pipenv con ```pip3 install pipenv```. Este "paquete" nos permitirá gestionar entornos virtuales.

<img width="1325" height="746" alt="image" src="https://github.com/user-attachments/assets/2bfaba6e-9626-4147-859a-8e66a02f6d55" />

Antes de continuar, nos aseguraremos de que pipenv se ha instalado correctamente ejecutando ```pipenv --version```. Aunque poniendo solamente el comando en sí este funciona a la primera,
en mi caso he tenido que poner justo antes del comando "~/.local/bin/" para que se pudiera ejecutar correctamente. Si no lo hacía, bash interpretaba "pipenv" como un comando a secas, no lo llegaba a encontrar y decía que no encontraba ese comando.

<img width="1323" height="744" alt="image" src="https://github.com/user-attachments/assets/f6804a75-dd7d-412c-9057-586bb8d31683" />

3. Ahora, nos descargaremos python-dotenv para cargar las variables de entorno ejecutando ```pip3 install python-dotenv```

<img width="994" height="698" alt="image" src="https://github.com/user-attachments/assets/d355f97d-e483-4822-b32e-5250c7d791d4" />
