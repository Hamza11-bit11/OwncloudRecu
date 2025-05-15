# Instal·lació i configuració d'aplicacions web

Per instal·lar una aplicació web hem de baixar el seu codi font i portar-lo al directori arrel del nostre servidor d'aplicacions, en el nostre cas, `apache2`. Quan instal·lem `apache2` es crea una carpeta a `/var/www/html` on, per defecte, el servidor web utilitza com a directori arrel.

Llavors, si portem la nostra aplicació al directori `/var/www/html` tindrem accés a la nostra aplicació mitjaçant l'adreça `http://localhost`.

## 📦 Instal·lació i Configuració d'Aplicacions Web (Apache2 + MySQL + PHP)

Aquest README t’ajuda a instal·lar i configurar una aplicació web pas a pas dins d’un contenidor amb Ubuntu. El servidor web serà Apache2 i farem servir MySQL com a base de dades.

1. Actualització de la màquina.
```console
sudo apt update
```
```console
sudo apt upgrade
```

2. Instal·lació del servidor web `apache2`.
```console
sudo apt install -y apache2
```

3. Instal·lació del servidor de bases de dades `mysql-server`.
```console
sudo apt install -y mysql-server
```

4. Instal·lació d'algunes llibreries de `php`, el llenguatge principal que utilitzen les aplicacions.
```console
sudo apt install -y php libapache2-mod-php
```
```console
sudo apt install -y php-fpm php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-ldap php-zip php-curl
```

5. Reiniciem el servidor apache2
```console
sudo systemctl restart apache2
```

## Configuració de MySQL
### Accedim a la consola de MySQL
Des d'un terminal on siguem `root` hem d'executar la següent comanda:
```console
sudo mysql
```

### Creació de la base de dades:
Un cop dins la consola de MySQL executem les comandes per a crear la base de dades. En aquest cas estem creant una base de dades amb el nom `bbdd`.

```console
CREATE DATABASE bbdd;
```

### Creació d'un usuari
Tingueu en compte que s'haurà d'identificar la IP des de la qual s'accedirà a la base de dades, en aquest cas, `localhost`.

```console
CREATE USER 'usuario'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

### Donem privilegis a l'usuari:
```console
GRANT ALL ON bbdd.* to 'usuario'@'localhost';
```

### Sortim de la base de dades
```console
exit
```

### Probem la connexió a la base de dades
Des d'un terminal amb un usuari sense privilegis hem de ser capaços de connectar introduïnt la nostra contrassenya.

```console
mysql -u usuario -p
```

## Extra: permetre la connexió des d'una màquina remota
Per seguretat, MySQL no permet per defecte connexions que no siguin des de localhost. Si volem canviar aquest comportament hem de crear un altre usuari que accedirà des d'una màquina remota i estarà identificat pel nom d'usuari i la seva IP. Així doncs, poden existir diferents usuaris anomenats `usuario` que connecten des de diferents màquines.

### Canviem l'accés per defecte a la nostra màquina
Permetem l'accés des de qualsevol equip a la nostra base de dades. Editem l'arxiu `/etc/mysql/mysql.conf.d/mysqld.cnf`

```console
vim /etc/mysql/mysql.conf.d/mysqld.cnf
```
![imatge](https://github.com/user-attachments/assets/fd240b56-5fc8-421f-9415-3a2bb8a92674)

Busquem la línia següent:
```console
bind-address = 127.0.0.1
```
![imatge](https://github.com/user-attachments/assets/6b07f688-250a-4b24-b9ef-24d927a1aa5d)

Hem de canviar el `bind-address` per `0.0.0.0` i la línia ha de quedar així:
```console
bind-address = 0.0.0.0
```
![imatge](https://github.com/user-attachments/assets/885666fe-4f3b-4fdd-b00e-82e4f38723e6)

### Reiniciem el servidor
```console
systemctl restart mysql
```
![imatge](https://github.com/user-attachments/assets/2905c8fb-342d-433a-a962-759c69c6b6c5)


### 🌐 Configuración de Acceso a la Nube para 2 Usuarios

#### 📁 Archivo Modificado
Se ha editado el archivo de configuración:


### Creació d'un usuari per a accedir des d'una màquina remota
Per accedir des d'una màquina remota, hauriem de crear un usuari nou identificat pel nom d'usuari i la IP de la màquina des de la qual accedirà.

```console
CREATE USER 'usuario'@'192.168.22.100' IDENTIFIED WITH mysql_native_password BY 'password';
```

Hem de donar privilegis a l'usuari que accedirà des de la màquina remota.
Per accedir des de fora, hauriem de donar-li també privilegis a l'usuari a l'altra màquina:

```console
GRANT ALL ON bbdd.* to 'usuario'@'192.168.22.100';
```

```console
exit
```
![imatge](https://github.com/user-attachments/assets/dc48fbb5-89c7-46de-8d8f-6c1183f1a666)


## Descarreguem els fitxers de l'aplicació web
Anem al directori `/var/www/html` i descomprimim allà els fitxers de l'aplicació web, heu de substituir `app-web.zip` per el nom del vostre fitxer que heu descarregat amb l'aplicació web i el nom de la carpeta `app-web` per la carpeta que us ha creat, si la vostra instal·lació de linux està en un idioma diferent al català, no tindreu la carpeta `Baixades`, modifiqueu la comanda per adaptarla a les vostrs necessitats.

```console
sudo cp ~/Baixades/app-web.zip /var/www/html
```
Aneu al directori `/var/www/html`
```console
cd /var/www/html
```
Descomprimiu el fitxer que heu baixat
```console
sudo unzip app-web.zip
```
Copieu els fitxers a la carpeta `/var/www/html`, modifiqueu `app-web` pel nom del directori on s'ha descomprimit el vostre arxiu.
```console
sudo cp -R app-web/. /var/www/html
```
Eliminem la carpeta creada quan hem fet l'`unzip`
```console
sudo rm -rf app-web/
```

## Eliminem el fitxer `index.html` de l'`apache2`
```console
sudo rm -rf /var/www/html/index.html
```

## Aplicació de permisos a les nostres aplicacions web
Un cop descomprimits els fitxers de l'aplicació web al directori `/var/www/html`, apliquem els següents permisos al directori `/var/www/html`

```console
cd /var/www/html
```
```console
sudo chmod -R 775 .
```
```console
sudo chown -R usuario:www-data .
```
## Accedim al navegador per veure que tot funciona
Poseu la direcció http://localhost al navegador web i configureu la cloud.

Si tot ha anat bé i heu seguit el manual us apareixerà l'instal·lador de l'aplicació web que heu baixat i us demanarà crear un usuario admin i la informació de la base de dades.

La informació que heu de posar (si no heu modificat la informació del manual) és la següent:

* **usuari:** usuario
* **contrasenya:** password
* **base de dades:** bbdd
* **domini:** localhost


# 🌥️ ownCloud - Demostración de Funcionamiento

Este repositorio documenta la demostración práctica del uso básico de **ownCloud** como plataforma de almacenamiento, colaboración y gestión de usuarios.

---

## ✅ Demostración del Funcionamiento

### 🔗 Compartir Contenidos
- Seleccionar un archivo o carpeta.
- Hacer clic en **"Compartir"**.
- Opciones disponibles:
  - Compartir con usuarios internos.
  - Crear enlace público con permisos configurables (lectura, escritura, fecha de expiración, contraseña).
- Verificar el acceso desde otra cuenta.

- ### 🔒 Pasos para compartir con caducidad:
1. Seleccionar un archivo o carpeta.
2. Clic en el icono de **"Compartir"**.
3. Activar la opción **"Compartir enlace público"**.
4. Configurar:
   - ✅ Permisos (lectura, edición, subida).
   - 📆 **Fecha de caducidad** (obligatoria para mayor seguridad).
   - 🔑 Contraseña (opcional pero recomendable).
5. Copiar el enlace generado y enviarlo al destinatario.
6. Verificar que, tras la fecha establecida, el enlace deja de estar activo automáticamente.
![imatge](https://github.com/user-attachments/assets/ecb61ef0-b55f-4ae7-a114-5da40579b2da)
### 📁 Creación de Carpetas
- Desde el botón **"+"**, seleccionar **"folder"**.
- Añade la carpeta

![imatge](https://github.com/user-attachments/assets/f3b3de4b-a468-4604-93e4-42bee8f3ccc9)

## 👤 Creación de Usuarios

Se han creado tres usuarios con distintos roles y permisos:

| Usuario   | Grupo           | Rol           |
|-----------|------------------|----------------|
| `admin`  | administradores  | Administrador  |
| `editor` | editores         | Editor         |
| `visualizador` | visualitzadors   | Visualizador   |

### 🔧 Proceso de Creación
1. Iniciar sesión como administrador.
2. Ir a **Configuración > Usuarios**.
3. Clic en **"Crear nuevo usuario"**.
4. Introducir nombre de usuario, contraseña y grupo asignado.

---

## 🔐 Asignación de Roles y Permisos

### 📜 Permisos por Rol
- **Administrador**: acceso total, gestión de configuración y usuarios.
- **Editor**: puede subir, editar y eliminar archivos.
- **Visualizador**: solo puede visualizar y descargar.

### 🧪 Prueba de Accesos
1. Compartir una carpeta con cada usuario con diferentes permisos.
2. Verificar:
   - El editor puede modificar archivos.
   - El visualizador solo puede leerlos.

![imatge](https://github.com/user-attachments/assets/f5fcfd4a-64d1-4e3e-919d-66934b8dd2bf)
![imatge](https://github.com/user-attachments/assets/5e4e7da9-8e24-4476-a640-2f030dd5d956)
