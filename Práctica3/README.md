# Práctica 3. Introducción a GKE

## Objetivo de la práctica:
Al finalizar la práctica, serás capaz de:
- Crear un Cluster de Kubernetes utilzando GKE autopilot.
- Uso basico del cluster.
- Desplegar contenedores en tu cluster de Kubernetes.

## Duración aproximada:
- 95 minutos.

## Instrucciones 

### Tarea 1. Ingresar a nuestra Consola en Google Cloud y configurar el proyecto en Cloud Shell.
Paso 1. Abrir la Consola de Google, desde <a href="https://console.cloud.google.com/">aquí</a>

Paso 2. Iniciar sesión.

Paso 3. Abrir Cloud Shell.

![open-shell](img/activate-shell.png)

Paso 4. Verificar que la cuenta esta siendo usada en Cloud Shell con el comando:

```
    gcloud auth list
```

Deberia aparecer la cuenta activa. En caso de que no aparezca o salga una cuenta diferente, utiliza el comando:

```
gcloud config set acocunt `Tu cuenta`
```
Paso 5. Verificar que estes trabajando en el proyecto correcto con el comando:

```
gcloud config list project
```

Puedes encontrar el ID del proyecto en la parte superior izquierda, al hacer click en el nombre del proyecto. Si el ID es distinto, utiliza el comando:

```
gcloud config set project `ID-del-proyecto`
```
### Tarea 2. Descargar el codigo muestra
Paso 1. En Cloud Shell utilizar el siguiente comando:

```
gsutil cp -r gs://spls/gsp021/* .
```

Paso 2. Cambiar al directorio "orchestrate-with-kubernetes/kubernetes". Utilizar el siguiente comando:

```
cd orchestrate-with-kubernetes/kubernetes
```

### Tarea 3. Crear el cluster de Kubernetes
Paso 1. Crear tu primer cluster de Kubernetes. En Cloud Shell utilizar en comando:

```
gcloud container clusters create k1 --zone us-west4
```

Esto puede demorar hasta 15 minutos.

Paso 2. Configurar .kube file. Utilizar el comando:

```
gcloud container clusters get-credentials k1
```

### Tarea 4. Crear los primeros objetos de Kubernetes
Antes de desplegar nuestra app tengamos un poco de práctica con Kubernetes y Kubectl.

Paso 1. Lanzar una instancia de un contenedor de nginx. Utilizar el comando:

```
kubectl create deployment nginx --image=nginx:1.10.0
```

Paso 2. Verificar que exista el pod ejecutando el contenedor de nginx. Utilizar el comando:

```
kubectl get pods
```

Paso 3. Una vez se encuentre ejecutandose el pod, exponerlo fuera de Kubernetes. Utilizar el comando:

```
kubectl expose deployment nginx --port 80 --type LoadBalancer
```

Paso 4. Observar la lista de servicios que tienes desplegados. Utilizar el comando:

```
kubectl get services
```

Paso 5. Hacer un ping al contenedor de manera remota. Utilizar el comando:

```
curl http://<IP Externa>:80
```
Puedes encontrar la IP, en el paso anterior (Paso 4).

Apartir de aqui empezaremos a trabajar con nuestro código muestra.

### Tarea 5. Explorar el código de la aplicación

Date unos minutos para explorar el código muestra de la aplicación, sobre todo pon atención a los archivos YAML.

### Tarea 6. Utilizar los archivos de configuración para desplegar Pods

Paso 1. Asegurate de estar en el directorio "*orchestrate-with-kubernetes/kubernetes*". 

```
cd ~/orchestrate-with-kubernetes/kubernetes
```

Paso 2. Visualizar el archivo monolith.yaml que se encuentra en la carpeta pods. Utilizar el comando:

```
cat pods/monolith.yaml
```

Paso 2. Crear el pod monolith. Utilizar el comando:

```
kubectl create -f pods/monolith.yaml
```

Paso 4. Verificar el estado de los pods. Utilizar el comando:

```
kubectl get pods
```

Paso 5. Obtener información adicional del pod. Utilizar el comando:

```
kubectl describe pods monolith
```

### Tarea 7. Crear un servicio para acceder al pod
Paso 1. Asegurate de estar en el directorio "*orchestrate-with-kubernetes/kubernetes*". 

```
cd ~/orchestrate-with-kubernetes/kubernetes
```

Paso 2. Desplegar pods con el archivo de configuracion secure-monolith.yaml, asegurate de crear un pod seguro con datos de configuración. Utilizar los siguientes comandos:

```
kubectl create secret generic tls-certs --from-file tls/
```

```
kubectl create configmap nginx-proxy-conf --from-file nginx/proxy.conf
```

```
kubectl create -f pods/secure-monolith.yaml
```

Paso 3. Explorar la configuración del servicio. Utilizar el comando:

```
cat services/monolith.yaml
```

Paso 4. Crear el servicio con el archivo de configuración "*monolith.yaml*" que se encuentra en la carpeta Services. Utilizar el comando:

```
kubectl create -f services/monolith.yaml
```

Paso 5. Configurar una regla de firewall para permitir tráfico hacia tu servicio. Utilizar el comando:

```
gcloud compute firewall-rules create allow-monolith-nodeport \
  --allow=tcp:31000
``` 
Generalmente esto lo hace Kubernetes por nosotros. Sin embargo, estamos eligiendo un puerto en particular para configurar health checks de manera mas sencilla si asi quisieramos.

Paso 6. Obtener la dirección IP externa para alguno de los nodos. Utilizar el comando:

```
gcloud compute instances list
```

Paso 7. Intentar hacer un pin al *secure-monolith*. Utilizar el comando:

```
curl -k https://<IP EXTERNA>:31000
```
Podras notar que hay un timeout, esto es debido a las etiquetas, los pods no tienen las etiquetas que utilizamos en nuestro servicio.

### Tarea 8. Añadir etiquetas a los pods
Paso 1. Verificar los pods ejecutandose con la etiqueta "app=monolith". Utilizar el comando:

```
kubectl get pods -l "app=monolith"
```

Paso 2. Verificar los pods ejecutandose con la etiqueta "secured=enabled". Utilizar el comando:

```
kubectl get pods -l "secured=enabled"
```
Podras notar que no encuentras ningún pod.

Paso 3. Añadir las etiquetas faltantes. Utilizar el comando:

```
kubectl label pods secure-monolith 'secure=enabled'
```

Paso 4. Verificar que las etiquetas se hayan actualizado. Utilizar el comando:

```
kubectl get pods secure-monolith --show-labels
```

Paso 5. Vizualizar la lista de *Endpoints* del servicio. Utilizar el comando:

```
kubectl describe services monolith | grep Endpoints
```

Paso 6. Intenta hacer un ping a el pod. Utilizar el comando:

```
curl -l https://<IP EXTERNA>:31000
```
Deberías poder observar que si hay contacto con el pod.

### Tarea 9. Separar la aplicación monolitica a microservicios
Ahora que ya tuviste practica desplegando Pods toca practicar desplegando "Deployments"

Paso 1. Examinar la configuración del archivo "auth.yaml". Utilizar el comando:

```
cat deployments/auth.yaml
```

Paso 2. Crear un objeto deployment basado en el archivo auth.yaml. Utilizar el comando:

```
kubectl create -f deployments/auth.yaml
```

Paso 3. Crear un servicio para el deployment. Utilizar el comando:

```
kubectl create -f services/auth.yaml
```

Paso 4. Repetir los mismos pasos para los archivos *hello.yaml*. Los puedes encontrar en la carpeta deployments y services.

Paso 5. Repetir los mismos pasos para los archivos *frontend.yaml*. Los puedes encontrar en la carpeta deployments y services. Pero primero utiliza el comando:

```
kubectl create configmap nginx-frontend-conf --from-file=nginx/frontend.conf
``` 
Este paso es un configmap que requiere el contenedor del frontend.yaml.

Paso 6. Obtener la IP externa del servicio frontend. Utilizar el comando:

```
kubectl get services frontend
```

Paso 7. Hacer un ping al servicio. Utilizar el comando:

```
curl -k https://<IP EXTERNA>
```



### Resultado esperado
![imagen resultado](img/resultado.png)

¡Felicidades! Con esto haz concluido tu tercer laboratorio. 
No olvides solicitar ayuda a tu entrenador para eliminar los recursos que recien creaste.
