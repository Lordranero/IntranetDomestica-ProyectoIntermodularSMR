# 📓 Diario de Desarrollo: [Intranet Doméstica]

> 👥 **Autores:** Álvaro Serrano

> **Descripción corta:** El proyecto escogido consiste en el desarrollo de una red privada, con sus pertinentes servicios y herramientas, que funcione tanto para el entorno doméstico como para el profesional; una intranet doméstica en toda regla.

> 📅 **Fecha de inicio:** [07/10/2026]

> 🚀 **Estado actual:** Versión Alfa

---

## 📌 Tabla de Contenidos
* [Visión General y Tecnologías](#-MVP-y-Visión-General)
* [Bitácora](#-Bitácora)
* [Problemas Conocidos y Soluciones](#-Problemas-Conocidos-y-Soluciones)

---

## 🛠️ MVP y Visión General

* **Red segmentada.** Se dispondrá de una red que separe los dispositivos en varios grupos: red de invitados, taller, domótica, …
* **Túnel VPN.** Usando la tecnología nativa de Wireguard (más adelante se especifica el router).
* **Bloqueo y cifrado por DNS.** Mediante tecnología DNS-over-HTTPS y estableciendo listados.
* **Configuración del Firewall.** Filtrando el tráfico entre la red privada e internet o entre las VLANs.
* **Registro y análisis del tráfico.** Mediante paquetes de OpenWRT y el almacenamiento a través de un servicio de Syslog externo al router.
* **Empaquetado de servicios.** No todo cabe ni debe de estar en el router por lo que se usará un dispositivo con Docker para ciertos servicios.

Quedará limitado al domicilio: no busco contratar un VPS, crear un dominio, negociar o pedir nuevos servicios a mi ISP, etc. La intención es hacer todo lo que se pueda desde mi propio router y mi estación de trabajo.

### Stack Tecnológico
* **Dispositivos:** GL-MT6000, Raspberry PI 5

---

## 📝 Bitácora

<!--
### 📅 [DD/MM/AAAA] - [Motivo]
* **Avances:**
* **Retos / Bloqueos:** 
-->

### 📅 [13/10/2026] - [Router: Instalación y configuración Inicial]
* **Avances:**

Tras buscar un hueco y conectar los distintos dispositivos necesarios para el proyecto, accedí a la dirección del router para llevar a cabo las configuraciones iniciales; en el primer inicio de sesión, el router solicita un cambio de contraseña (la cual generé en una kbdx, base de datos para contraseñas, en keepassxc).

<p align="center">
  <img src="assets/img/13-09/Router&Raspberry.jpeg" width="200" style="transform: rotate(90deg);">
</p>

Una vez en el panel, los primeros pasos fueron especificar las credenciales otorgadas por mi proveedor de internet, aquí tuve que especificar la VLAN dentro de la red de mi ISP y las credenciales reservadas por contrato.

<div align="center">
 <table>
   <caption>Paneles de bienvenida</caption>
   <tr>
     <td><img src="assets/img/13-09/1st_iniRouter-1.png" width="300"></td>
     <td><img src="assets/img/13-09/1st_iniRouter-2.png" width="300"></td>
     <td><img src="assets/img/13-09/1st_iniRouter-4.png" width="200"></td>
   </tr>
 </table>
</div>

A continuación, tras verificar la conexión y hacer pruebas de velocidad, procedí a cambiar el DNS de mi ISP por el de Cloudflare y habilitar la encriptación vía DoT; esta primera configuración es provisional y se actualizará con la instalación de una VPN (WARP de Cloudflare). El objetivo final de este apartado es garantizar una conexión segura, privada y rápida.

<div align="center">
 <table>
   <caption>Configuración del DNS</caption>
   <tr>
     <td><img src="assets/img/13-09/DNSChange-POST.png" width="300"></td>
     <td><img src="assets/img/13-09/DNSChange-DebugInfo.png" width="200"></td>
   </tr>
 </table>
</div>

Por último cambié el SSID (para que no figurara el modelo de mi router como nombre de red), establecí una nueva contraseña mediante KeepassXC y oculté la red.

<div align="center">
 <table>
   <caption>Personalización del Wi-Fi</caption>
   <tr>
     <td><img src="assets/img/13-09/WirelessOptions.png" width="300"></td>
     <td><img src="assets/img/13-09/NetworkPersonalizada.png" width="300"></td>
   </tr>
 </table>
</div>

* **Retos / Bloqueos:** 
  * La conexión a internet del router no funcionaba en primera instancia por la PPPoE, tuve que solicitar las credenciales al servicio técnico de mi ISP.

<p align="center">
  <br>
  <img src="assets/img/13-09/CredencialesPPPoE.png" width="300">
</p>

---

### 📅 [19/10/2026] - [SBC: Instalación y configuración Inicial]
* **Avances:**

Descargué tanto el cliente de <a href="https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html">**PuTTY**</a> como el grabador de imágenes <a href="">**Raspberry Pi Imager**</a> desde las páginas oficiales de ambos e instalé siguiendo el proceso convencional.



<div align="center">
 <table>
   <caption>Descarga e instalación de PuTTY y RBPi Imager</caption>
   <tr>
     <td><img src="assets/img/19-09/PuTTY_02-install.png" width="300"></td>
     <td><img src="assets/img/19-09/PuTTY_01-download.png" width="300"></td>
     <td><img src="assets/img/19-09/RBPi_01-donwloadImager.png" width="300"></td>
   </tr>
 </table>
</div>

Una vez preparados los programas, introduje la microtarjeta SD, destinada a ser el cerebro del SBC, en un adaptador USB para poder grabar el sistema operativo; la memoria para los registros, y otras funciones que requieran escritura constante, irán en una SSD debido a la degración que sufriría la micro SD. 

El SO escogido para la Raspberry ha sido DietPi debido a: **un consumo mínimo de recursos**, ocupa menos de medio GiB y consume apenas 50 MiB de RAM en reposo; **una mayor vida útil para la tarjeta SD**, incluye un sistema de registro en memoria RAM (DietPi-RAMlog) que evita escrituras constantes en la tarjeta; y por último, **una automatización completa desde el primer arranque**, permite configurar características del sistema e instalar *software* preconfigurado, antes del primer inicio, desde el archivo "dietpi.txt" situado en la partición de arranque.

<p align="center">
  <img src="assets/img/19-09/RBPi_02-burnImage.png" width="400" height="300";">
</p>



* **Retos / Bloqueos:** 

---

### 📅 [6/11/2026] - [Mapa de Red]
* **Avances:**

* **Retos / Bloqueos:** 

<!--
## ⚠️ Problemas Conocidos y Soluciones

| Error / Bug | Causa | Solución |
| :--- | :--- | :--- |
---
-->