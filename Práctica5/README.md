# Práctica 5. Introducción a Cloud Storage

## Objetivo de la práctica:
Al finalizar la práctica, serás capaz de:
- Crear instancias de Cloud SQL.
- Configurar réplicas de lectura.

## Duración aproximada:
- 40 minutos.

## Instrucciones 

### Tarea 1. Ingresar a la Consola en Google Cloud y configurar el proyecto en Cloud Shell.
Paso 1. Abrir la Consola de Google, desde <a href="https://console.cloud.google.com/">aquí</a>

Paso 2. Iniciar sesión.

Paso 3. Abrir Cloud Shell.

![open-shell](img/activate-shell.png)

Paso 4. Verificar que la cuenta esta siendo usada en Cloud Shell con el comando:

```
    gcloud auth list
```

Deberia aparecerte la cuenta activa. En caso de que no aparezca o salga una cuenta diferente, utiliza el comando:

```
gcloud config set acocunt `Tu cuenta`
```
Paso 5. Verificar que estes trabajando en el proyecto correcto con el comando:

```
gcloud config list project
```

Puedes encontrar el ID del proyecto en la parte superior izquierda al hacer click en el nombre del proyecto. Si el ID es distinto utiliza el comando:

```
gcloud config set project `ID-del-proyecto`
```
### Tarea 2. Acceder a Cloud SQL
Paso 1.  En el menú de navegación, desplazarse hacia abajo y hacer clic en "SQL".


### Tarea 3. Crear una instancia de Cloud SQL
Paso 1. Hacer click en *Crear instancia*. 

Paso 2. Seleccionar MySQL como motor para la base de datos.

- ID_DEL_PROYECTO-storageLab.

Paso 3. Configurar la instancia con las siguientes caracterizticas:

| Configuración | Valor | 
| Edición de SQL | Enterprise | 
| Preset | Sandbox |
| Version de la base de datos | MySQL 8.0 |
| Id de la instancia | my-first-sql-instance |
| Constraseña | *De tu preferencia* |


Dejar el resto en los predeterminados y hacer click en *Crear instancia*. Este proceso puede tardar de 10 a 15 minutos.


### Tarea 4. Conectarse a la instancia
Paso 1. Dentro de la pagína de detalles de la instancia, puedes encontrar su IP pública, guardala para usarla más delante.

![Ip_Publica](img/ip-publica.png)

Paso 2. Ir al apartado de usuarios.

Paso 3. Hacer click en añadir cuenta de usuario y agregar un usuario con las siguientes caracterizticas:

| Configuración | Valor | 
| Nombre de usuario | userdb1 | 
| Contraseña | dbpassword1 |

Haz click en añadir.

Paso 4. Conectarse a la instancia por medio de Cloud Shell. Utilizar el comando:

```
gcloud sql connect my-first-sql-instance --user=userdb1
```
Utilizar la contraseña cuando se solicite.

### Tarea 5. Interactuar con la instancia
Paso 1. Crear una base de datos. Utilizar el siguiente comando:

```sql
CREATE DATABASE mi_base_datos;
```

Paso 2. Crear una tabla en la base de datos. Utilizar el siguiente codigo SQL:

```sql
CREATE TABLE usuarios (
      id INT AUTO_INCREMENT,
      nombre VARCHAR(100),
      email VARCHAR(100),
      PRIMARY KEY(id)
    );
```

Paso 3. Insertar datos en la base de datos. Utilizar el siguiente codigo:

```sql
INSERT INTO usuarios (nombre, email) VALUES ('Juan Pérez', 'juan@example.com');
```

Paso 4. Hacer una consulta de la base de datos. Utilizar el siguiente codigo:

```sql
SELECT * FROM usuarios;
```

Paso 5. Cerrar la conexión. Utilizar el comando:

```
exit
```

### Tarea 6. Configurar replicas de lectura.
Paso 1. Ir a la página de Cloud SQL y abrir los detalles de la instancia.

Paso 2. Ir al apartado de Replicas.

Paso 3. Hacer click en *crear replica de lectura*. Dejar todo por defecto y hacer click en *Crear replica*.

Paso 4. Conectarse a la instancia replica. Utilizar el comando:

```
gcloud sql connect my-first-sql-instance-replica --user=userdb1
```

Utilizar la contraseña cuando se solicite.

Paso 5. Realizar una consulta de los datos. Utilizar el código:

```sql
SELECT * FROM usuarios;
```

Paso 6. Cerrar la conexión. Utilizar el comando:

```
exit
```

### Tarea 7 (Opcional) (Challenge). Conectarse a la base de datos desde Workbench o un Cliente SQL similar
Paso 1. Utilizar la direccion IP pública para conectarse a la instancia replica desde Workbench o un Cliente SQL similar.

### Resultado esperado
![imagen resultado](img/resultado.png)

¡Felicidades! Con esto haz concluido tu quinto laboratorio. 
No olvides solicitar ayuda a tu entrenador para eliminar los recursos que recien creaste.
