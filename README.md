# Owncloud Recu
# Manual d'Instal·lació d’ownCloud amb IsardVDI

## 1. Instal·lació d’IsardVDI

### Requisits previs
- Sistema operatiu host: Ubuntu Server 20.04
- Accés a internet
- Privilegis de superusuari (sudo)

### Passos
1. **Actualització del sistema**
    ```bash
    sudo apt update && sudo apt upgrade
    ```

2. **Descàrrega i instal·lació d’IsardVDI**
    [Enllaç oficial d’instal·lació: https://docs.isardvdi.com]

    *(Afegeix aquí les captures de pantalla de cada pas)*

3. **Creació d’una màquina virtual**
    - Nom: `ownCloudVM`
    - Sistema operatiu: Ubuntu 20.04
    - Recursos: 2 vCPU, 4GB RAM, 20GB disc

## 2. Instal·lació d’ownCloud

### 1. Instal·la Apache, PHP i MariaDB
```bash
sudo apt install apache2 mariadb-server libapache2-mod-php php php-mysql php-xml php-curl php-zip php-mbstring php-gd php-intl unzip wget

![imatge](https://github.com/user-attachments/assets/c3a1f340-56ce-485a-a944-118edfa24fa2)
![imatge](https://github.com/user-attachments/assets/468083fc-5976-40d1-ae9f-770a92a2ed62)

