# Bitácora Técnica IV

## Laboratorio de Teletransportación Digital (SSH y RDP)

**Resultado de Aprendizaje:** RA6

Alumno: Elena Sáez Lascurain

**Criterios de Evaluación**:

d) Se ha accedido a los servidores utilizando técnicas de conexión remota.  
e) Se ha evaluado la necesidad de proteger los recursos y el sistema.  
f) Se han instalado y evaluado utilidades de seguridad básica.

# 1\. Introducción

El objetivo de este laboratorio es configurar accesos remotos seguros y aprender que el puerto 2222 es un imán para los problemas si no se protege.

Estaré usando **Docker Compose** para esta actividad.

## Tarea 1: Despliegue de la Infraestructura

1. Ejecuto *docker-compose up \-d* una vez creado el **docker-compose.yml** dentro de la carpeta llamada *SI\_Bitacora4\_ElenaSaez*.

   **Error encontrado:**  
   ![][image1]

   ¿Cómo lo he solucionado?

   Usando un comando de docker que refresca todas las Cookies y Caché innecesario que no me está permitiendo ejecutar el comando para montar mi Docker, **docker system prune**

   ![][image2]

   ![][image3]

   Pero me sigue saliendo el mismo error:  
      ![][image4]

   Entonces procedo a reiniciar el ordenador y ya me funciona.

   ![][image5]

2. Verifico que los contenedores están corriendo con ***docker ps***:

   

# Tarea 2: SSH y la clave pública

1. Me conecto al Docker usando *ssh alumno@localhost \-p 2222 en el shell*, usando la contraseña *sistemas\_informaticos*

   **Me da un error:** Al hacer esto me da una advertencia diciendo que la identificación del host ha cambiado y que tenga cuidado, no pudiendo acceder al siguiente paso para poner la contraseña.

   ![][image6]

   En vez de usar localhost, uso la IP y ya me funciona:

   ![][image7]

   Lo compruebo a través de *docker ps:*

   *![][image8]*

2. Genero un par de llaves en mi **máquina anfitriona:** *ssh-keygen \-t ed25519 \-C "elenasaez.25@campuscamara.es"*

   Este es el resultado del comando:

   *![][image9]*

![][image10]

3. Ahora copio esta llave pública al servidor usando el comando *ssh-copy-id \-p 2222 alumno@localhost*

   *![][image11]*

## Tarea 3: RDP \- Escritorio en mi navegador

1. Abro el cliente de mi Escritorio remoto utilizando MSTSC y pongo localhost:3389 para conectarme:

   ![][image12]

   

   

**Error encontrado:**

Como me falla con este error, voy a la dirección *http://localhost:3000* directamente y me abre el escritorio de Ubuntu gracias a Apache Guacamole.

![][image13]

2\. Para probarlo, creo un archivo de texto en el escritorio del contenedor llamado *PRUEBA\_LOGRADA.txt* con un mensaje para el profesor.

![][image14]

![][image15]

# 

# 4\. Reflexión final:

Pienso que SSH es más utilizado en servidores de producción que RDP porque:

1. **Superioridad en Seguridad**  
- **Cifrado de Extremo a Extremo:** SSH cifra toda la sesión, siendo altamente seguro por defecto. 

- **Autenticación basada en claves:** SSH permite el uso de pares de claves lo cual es mucho más seguro que las contraseñas usando RDP

- **Menor Superficie de Ataque:** Al no requerir una interfaz gráfica, SSH reduce vulnerabilidades


2. **Eficiencia y Rendimiento**  
- **Ligero y Rápido:** SSH funciona mediante una interfaz de línea de comandos consumiendo menos recursos de CPU y RAM

- **Funciona en redes lentas:** RDP necesita transmitir imágenes de escritorio, lo que requiere muchos más recursos, mientras que SSH funciona perfectamente sin tener que requerir tantos recursos.

# 

# 5\. Rúbrica de Calificación

| Criterio | Puntuación | Descripción |
| ----- | :---: | ----- |
| **Configuración SSH** | 40% | Llaves generadas, acceso sin pass y hardening aplicado. |
| **Acceso RDP** | 10% | Conexión establecida y evidencia gráfica en el escritorio. |
| **Documentación GitHub** | 40% | **README** claro, bien formateado y con capturas nítidas. Código bien ejecutado. |
| **Actitud y Resolución** | 10% | Capacidad para ayudar a compañeros y resolver dudas. |
| **Uso de la “IA”** para cualquier aspecto de la Bitácora. | **\-100%** |  |