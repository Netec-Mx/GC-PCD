# Práctica 1. Introducción a Cloud Shell

## Objetivo de la práctica:
Al finalizar la práctica, serás capaz de:
- Tener un primer acercamiento a Cloud Shell.
- Conocer commandos basicos para configurar Cloud Shell.
- Crea tu primer recurso de Compute Engine por medio de Cloud Shell.

## Duración aproximada:
- 15 minutos.

## Instrucciones 

### Tarea 1. Ingresar a la Consola en Google Cloud y configurar el proyecto en Cloud Shell.
Paso 1. Abrir la Consola de Google, desde <a href="https://console.cloud.google.com/">aquí</a>.

Paso 2. Iniciar sesión.

Paso 3. Abrir Cloud Shell.

![open-shell](activate-shell1.png)

Paso 4. Verificar que la cuenta esta siendo usada en Cloud Shell con el comando:

```
    gcloud auth list
```

Deberia aparecer como cuenta activa. En caso de que no aparezca o salga una cuenta diferente, utilizar el comando:

```
gcloud config set acocunt `Tu cuenta`
```
Paso 5. Verificar que estes trabajando en el proyecto correcto con el comando:

```
gcloud config list project
```

Puedes encontrar el ID del proyecto en la parte superior izquierda al hacer click en el nombre del proyecto. Si el ID es distinto, utilizar el comando:

```
gcloud config set project `ID-del-proyecto`
```


### Tarea 2. Configurar el entorno
Una vez que Cloud Shell esta listo para utilizarse o Cloud SDK, podemos empezar a configurar el entorno antes de empezar a crear recursos.

Paso 1. Configurar la región predefinida sobre la cual se crearan recursos. Utilizar el comando:

```
gcloud config set compute/region REGION
```
No olvides cambiar `REGION` por la región de tu preferencia o que indique tu instructor.

Paso 2. Verificar que la configuracion se guardo exitosamente con el comando:

```
gcloud config get-value compute/region
```

Paso 3. Configurar la zona predefinida para la creación de recursos en el proyecto. Utilizar el comando:

```
gcloud config set compute/zone ZONE
```
No olvides cambiar `ZONE` por la zona adecuada a tu región o la que indique tu instructor.

Paso 4. Verificar que la configuración se guardo exitosamente con el comando:

```
gcloud config get-value compute/zone
```

### Tarea 3. Configurar variables de entorno
Muchas veces existen ciertos valores que usamos con frecuencia mientras trabajamos con la linea de comandos, para facilitarnos el uso de estos valores podemos crear variables de entorno.

Paso 1. Crear la variable de entorno que guarda el ID del proyecto. Utilizar el siguiente comando:

```
export PROJECT_ID=$(gcloud config get-value project)
```

Paso 2. Verificar que la variable se guardo de forma exitosa:

```
echo $PROJECT_ID
```

Paso 3. Crear una variable de entorno para guardar una zona. Utilizar el siguiente comando:

```
export ZONE=`ZONE`
```
No olvides cambiar ZONE por la zona de tu preferencia o la que indique tu instructor.

Paso 4. Verificar que la variable se aguardo de forma exitosa:

```
echo $ZONE
```

### Tarea 4. Crear y manejar una maquina virtual
Cloud Shell y Cloud SDK son herramientas importantes para el manejo de recursos en Google Cloud, vamos a crear el primer recurso en la nube.

Paso 1. Crear una maquina virtual. Utilizar el siguiente comando:

```
glcoud compute instances create my-first-vm --machine-type e2-medium --zone $ZONE
```
La creación de la maquina virtual no debe tardar más de un minuto.

Paso 2. Verificar que la maquina virtual se haya creado. Utilizar el siguiente comando:

```
gcloud compute instances list
```

Paso 3. Añadir un filtro al listado. Utilizar el siguiente comando:

```
gcloud compute instances list --filter="name=('my-first-vm')"
```
Este filtro, nos muestra solo las maquinas virtuales que tengan ese nombre en particular. Hay varios filtros que podemos usar como zona, región y más, intenta crear tus propios filtros.

### Tarea 4. Conectate a la maquina virtual
Es posible hacer ssh o rdo a nuestras maquinas virtuales en Google Cloud.

Paso 1. Verificar las reglas de firewall en el proyecto. Utilizar el comando:

```
gcloud compute firewall-rules list
```

Dentro de la lista debemos tener un resultado llamado default-allow-ssh, si no se encuentra pide ayuda a tu instructor.

Paso 2. Hacer una conexion SSH a la maquina virtual. Utilizar el comando:

```
gcloud compute ssh my-first-vm --zone $ZONE
```

Cuando pregunte, escribir *y* y presionar enter. Igualmente, dejar el *passphrase* en blanco.

Paso 3. Una vez conectado a la maquina virtual, instalar nginx. Utilizar el comando:

```
sudo apt-install -y nginx
```

Paso 4. Terminar la conexión a la maquina virtual. Utilizar el comando:

```
exit
```

### Resultado esperado

![result](img/resultado1.png)

¡Felicidades! Con esto haz concluido tu primer laboratorio. 
No olvides solicitar ayuda a tu entrenador para eliminar los recursos que recien creaste.

