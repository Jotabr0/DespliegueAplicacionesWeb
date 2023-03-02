```

#!/bin/bash


# Obtener el nombre de usuario del nuevo cliente
read -p "Introduce el nombre de usuario: " username

# Comprobar si el usuario ya existe
if id "$username" >/dev/null 2>&1; then
  echo "El usuario $username ya existe" >&2
  exit 1
fi

# Crear el usuario con su directorio de inicio
useradd -m "$username"

# Establecer la contraseña del usuario
passwd "$username"

# Instalar el servidor FTP (vsftpd)
apt-get update
apt-get install vsftpd

# Crear un directorio para el usuario en FTP
mkdir -p "/home/$username/ftp"

# Establecer los permisos adecuados para el directorio
chown -R "$username":"$username" "/home/$username/ftp"
chmod -R 755 "/home/$username/ftp"

# Configurar vsftpd para permitir el acceso mediante TLS
cat >> /etc/vsftpd.conf <<EOF
ssl_enable=YES
rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
force_local_data_ssl=YES
force_local_logins_ssl=YES
ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO
require_ssl_reuse=NO
ssl_ciphers=HIGH
EOF

# Reiniciar vsftpd
systemctl restart vsftpd

# Habilitar el acceso SSH/SFTP
sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
systemctl restart sshd

echo "El usuario $username se ha creado con éxito"



```
