# Guia de configiracion y instalacion de servidor samba

Mofificar el archivo de directa empresa para agregar smtp

Directa lan

```hs
smtp IN     A   198.100.1.10
```

Inversa lan

```hs
smtp IN     A   198.100.1.10


10   IN     PTR  smtp.miempresa.ucv.
```

Lo mismo para POP y FTP

---
Instalar samba

```
yum -y install samba
```

Modificar archivos de configuracion

```
nano /etc/samba/smb.conf
```

Modificar en global
```
workgroup = WORKGROUP
```
Agregar en global
```
  domain logons = yes
  local master = yes
  preferred master = yes
  logon path = \\%L\Profiles\%U
  logon script = logon.bat
  add machine script = /usr/sbin/useradd -d /dev/null -g
```
Modificar en Homes
```
browseable = yes
```
Agregar en homes
```
writable = yes
```
Agregar en public
```
  path = /data/public
  browsable = yes
  writable = yes
  guest ok = yes
  read only = no
```

Creamos un nuevo usuario
```
// crear
useradd user_samba
// Agregar password al usuario
passwd user_samba
```

Indexamos al usuario al servidor samba

```
sambapasswd -a user_samba
```

Creamos el directorio que se mostrar y agregamos permisos

```
mkdir /home/archivo_samba
```
```
chmod 777 /home/archivo_samba
```
```
systemctl start smb nmb
```
```
systemctl enable smb nmb
```
```
firewall-cmd --add-service=samba --permanent
```
```
firewall-cmd --permanent --zone=public --add-service=samba
```
```
firewall-cmd --reload
```

