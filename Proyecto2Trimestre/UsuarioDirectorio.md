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

# Crear el directorio web del usuario
mkdir -p "/var/www/$username/public_html"

# Crear una página web por defecto
echo "<html><body><h1>Bienvenido a mi sitio web</h1></body></html>" > "/var/www/$username/public_html/index.html"

# Establecer los permisos adecuados para el directorio
chown -R "$username":"$username" "/var/www/$username/public_html"
chmod -R 755 "/var/www/$username/public_html"

# Crear un archivo de configuración para el sitio web
cat > "/etc/apache2/sites-available/$username.conf" << EOF
<VirtualHost *:80>
  ServerName $username.example.com
  DocumentRoot /var/www/$username/public_html
  <Directory /var/www/$username/public_html>
    AllowOverride All
  </Directory>
</VirtualHost>
EOF

# Habilitar el sitio web
a2ensite "$username.conf"

# Reiniciar Apache
systemctl restart apache2

echo "El usuario $username se ha creado con éxito"

```
