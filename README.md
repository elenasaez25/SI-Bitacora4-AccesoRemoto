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
   <img width="501" height="122" alt="image" src="https://github.com/user-attachments/assets/7d2d2114-59f4-4638-8725-76dbde2c4a88" />


   ¿Cómo lo he solucionado?

   Usando un comando de docker que refresca todas las Cookies y Caché innecesario que no me está permitiendo ejecutar el comando para montar mi Docker, **docker system prune**

   <img width="530" height="184" alt="image" src="https://github.com/user-attachments/assets/5c2067c7-9cec-47b9-9e23-13dd1035fc78" />


   <img width="269" height="69" alt="image" src="https://github.com/user-attachments/assets/433142f1-4dfd-4743-b5d4-eda48568a31e" />


   Pero me sigue saliendo el mismo error:  
      <img width="498" height="204" alt="image" src="https://github.com/user-attachments/assets/6bb70ac9-2f37-4edd-8bfa-8b57e85121b2" />


   Entonces procedo a reiniciar el ordenador y ya me funciona.

   <img width="1610" height="310" alt="image" src="https://github.com/user-attachments/assets/b3bb8d9c-84b5-422e-b179-96d4d761858a" />


2. Verifico que los contenedores están corriendo con ***docker ps***:
<img width="1456" height="83" alt="image" src="https://github.com/user-attachments/assets/63b950e6-7ac4-4279-aa1d-951571ae9239" />

   

# Tarea 2: SSH y la clave pública

1. Me conecto al Docker usando *ssh alumno@localhost \-p 2222 en el shell*, usando la contraseña *sistemas\_informaticos*

   **Me da un error:** Al hacer esto me da una advertencia diciendo que la identificación del host ha cambiado y que tenga cuidado, no pudiendo acceder al siguiente paso para poner la contraseña.

   <img width="1010" height="304" alt="image" src="https://github.com/user-attachments/assets/3fd495ba-6c00-47b2-8551-ad35d5ec177e" />


   En vez de usar localhost, uso la IP y ya me funciona:

   <img width="630" height="153" alt="image" src="https://github.com/user-attachments/assets/88301e5d-72af-4ee6-994f-2a1fc6a3e9dd" />


   Lo compruebo a través de *docker ps:*

   <img width="1433" height="81" alt="image" src="https://github.com/user-attachments/assets/2698bfa6-8714-46ec-b17c-9be3d8a130ed" />


2. Genero un par de llaves en mi **máquina anfitriona:** *ssh-keygen \-t ed25519 \-C "elenasaez.25@campuscamara.es"*

   Este es el resultado del comando:

   <img width="582" height="276" alt="image" src="https://github.com/user-attachments/assets/104573a2-1492-4fb9-a5db-daebfaef15b7" />

<img width="212" height="201" alt="image" src="https://github.com/user-attachments/assets/f16ca349-470a-4ae4-a938-e9af1a118a59" />


3. Ahora copio esta llave pública al servidor usando el comando *ssh-copy-id \-p 2222 alumno@localhost*

   <img width="860" height="270" alt="image" src="https://github.com/user-attachments/assets/bc9e5b3c-ff08-4d64-b5bf-3eb86042dcbe" />


## Tarea 3: RDP \- Escritorio en mi navegador

1. Abro el cliente de mi Escritorio remoto utilizando MSTSC y pongo localhost:3389 para conectarme:

   <img width="395" height="234" alt="image" src="https://github.com/user-attachments/assets/bd3f9a49-7122-4c40-be16-4c539fb489ef" />

**Error encontrado:**

Como me falla con este error, voy a la dirección *http://localhost:3000* directamente y me abre el escritorio de Ubuntu gracias a Apache Guacamole.

<img width="553" height="241" alt="image" src="https://github.com/user-attachments/assets/d15aba0e-e61d-4ec5-9ab5-38239d37cc8d" />



2\. Para probarlo, creo un archivo de texto en el escritorio del contenedor llamado *PRUEBA\_LOGRADA.txt* con un mensaje para el profesor.

<img width="298" height="147" alt="image" src="https://github.com/user-attachments/assets/5d6d1c1d-c7e6-42c1-bfa4-b47e7227abf4" />
<img width="847" height="542" alt="image" src="https://github.com/user-attachments/assets/5bc6654f-4649-4983-b2da-205f54ce10b0" />

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
