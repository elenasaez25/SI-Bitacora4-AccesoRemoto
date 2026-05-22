# 

# UD07. Elaboración de documentación técnica y uso de aplicaciones de propósito general

**Fecha:** 15 de mayo de 2026

**Alumno:** Elena Sáez Lascurain

**Ciclo:** Desarrollo de aplicaciones Web

---

# Índice {#índice}

[**Índice**](#índice)

[**Análisis de Necesidades**](#título-1)

[**El problema que resuelve esta arquitectura**](#título-2) 

[**1. Falta de centralización**](#título-3)  
[**2. Exposición de la superficie de ataque**](#título-3.1)  
[**3. Complejidad operativa**](#título-3.2)

[**¿Por qué Docker?**](#título-2.1)

[**Conclusión**](#título-2.2)

--- 

### **Análisis de Necesidades** {#título-1} 
En este análisis, voy a estar respondiendo a las cuestiones sobre las problemáticas que resolvemos utilizando Guacamole y Docker y por qué elegimos estas soluciones y no conectamos directamente por RDP a cada máquina:

## El problema que resuelve esta arquitectura {#título-2}

Uno de los principales problemas en los entornos empresariales con muchas máquinas virtuales es la gestión del acceso remoto y asegurarse que es segura. La práctica habitual de exponer el protocolo RDP de cada máquina a la red interna implica problemas que, a largo plazo, suponen un riesgo técnico importante.

---

### 1. Falta de centralización {#título-3}

Cuando cada equipo tiene su propio servicio RDP, no existe un punto de control único desde el que gestionar quién accede, cuándo y dónde. Esto dificulta la eliminación de permisos y el seguimiento de la actividad.

Con **Apache Guacamole** desplegado sobre Docker, toda la gestión se centraliza en un único portal web desde el que se administran conexiones, usuarios y grupos, sin distribuir los datos por cada máquina.

---

### 2. Exposición de la superficie de ataque {#título-3.1}

Abrir el puerto RDP (`3389`) en cada máquina multiplica las oportunidades de ataque. Basta con una credencial comprometida para que el riesgo se extienda.

Con Guacamole, solo se expone un único servicio **HTTPS** al exterior. Las conexiones RDP, SSH o VNC hacia las máquinas internas se realizan desde el propio servidor Guacamole, manteniendo los endpoints internos aislados de la red pública.

---

### 3. Complejidad operativa {#título-3.2}

Gestionar accesos de forma distribuida obliga a cada usuario a instalar un cliente, recordar distintas IPs y gestionar credenciales distintas. Guacamole elimina esta dependencia ofreciendo acceso desde cualquier navegador moderno, sin software adicional.

---

## ¿Por qué Docker? {#título-2.1}

Con **Docker Compose**, los componentes de Guacamole, la aplicación web y la base de datos se definen en un fichero declarativo, garantizando mejor portabilidad. El aislamiento por contenedores añade una seguridad de más, ya que cada servicio opera con los permisos mínimos.

---

## Conclusión {#título-2.2}

Guacamole con Docker resuelve la dispersión del control de accesos, reduce la superficie de cualquier tipo de ataque y además elimina la dependencia de clientes específicos. Frente al RDP directo máquina a máquina, esta arquitectura representa una característica fundamental en la seguridad.

## 2. Estimación de Costes de Infraestructura  
En esta imágen se analiza una tabla profesional que calcula el coste mensual de alojar nuestra aplicación, incluyendo el cómputo, almacenamiento y transferencia de red, todo seguido del cálculo del subtotal y el IVA (21%) para obtener el **Total Mensual.**
<img width="806" height="195" alt="image" src="https://github.com/user-attachments/assets/3a569054-5840-4c35-bcd3-549257642ce1" />

## 3. Estrategia de Despliegue y Comunicación  

Para el despliegue de la aplicación en el servidor de producción utilizaremos **SFTP (SSH File Transfer Protocol)** como protocolo de transferencia de ficheros. A diferencia del FTP tradicional, que transmite datos en texto plano sin ningún tipo de cifrado, SFTP cifra tanto las credenciales como los datos durante la transferencia mediante el protocolo SSH, garantizando así la integridad de la información recibida, siendo esta la opción más segura y óptima para mover el código y archivos sensibles entre el entorno de desarrollo local y el servidor remoto.

A parte de esto, también estaremos llevando a cabo el uso de **integraciones Cloud nativas** como GitHub Actions, que permite automatizar el despliegue directamente desde el repositorio de código sin necesidad de transferir ficheros manualmente.  
Descartamos completamente el uso del **FTP tradicional** al ser un protocolo obsoleto que transmite datos sin cifrar, suponiendo un riesgo de seguridad inaceptable en entornos de producción.

### Mensajería

El equipo utilizará **Discord** como herramienta principal de mensajería para la coordinación técnica del proyecto. Se configurarán canales de chat específicos por área y se integrarán bots de alertas automáticas que permitan escribir en el canal correspondiente, de forma que si el servidor cae o se produce un error crítico, el equipo recibirá una notificación inmediata. Esto permite una respuesta rápida ante incidencias sin necesidad de estar monitorizando el servidor manualmente.

## 4. Justificación Científica

La relación de la conclusión del artículo escogido sobre **la seguridad en Docker** con mi proyecto es que, la gran cantidad de procesos que se pueden llegar a automatizar gracias a esta tecnología es universal. Esta tecnología aporta la libertad de poder revisar si los procesos instalados son vulnerables, y poder monitorizar en tiempo real si estos contenedores sufren cualquier tipo de ataque externo.  
Este artículo apoya mi proyecto de manera de que, al haber estado utilizando la tecnología **Docker** y comprobar que otro usuario también la empleó en su proyecto, me aseguro de que el propósito de uso de la tecnología la he empleado correctamente.

## Referencias (IEEE)
[1] IEEE: Xavier Fernández Ginés, "Seguridad en Docker", Universitat Oberta de Catalunya, Trabajo Final de Máster. Disponible en: https://openaccess.uoc.edu/server/api/core/bitstreams/9ab036b6-a555-4c09-8023-7772c25f935a/content [Accedido el 22/05/2026]

