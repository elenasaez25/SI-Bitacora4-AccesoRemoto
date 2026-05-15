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




