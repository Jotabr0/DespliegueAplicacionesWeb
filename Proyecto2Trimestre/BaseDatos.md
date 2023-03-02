

```

#!/bin/bash



# Obtener el nombre de la base de datos y del usuario
read -p "Introduce el nombre de la base de datos: " dbname
read -p "Introduce el nombre del usuario: " username
read -s -p "Introduce la contraseña del usuario: " password

# Crear la base de datos
mysql -u root -p -e "CREATE DATABASE $dbname;"

# Crear el usuario y otorgar permisos
mysql -u root -p -e "CREATE USER '$username'@'localhost' IDENTIFIED BY '$password';"
mysql -u root -p -e "GRANT ALL PRIVILEGES ON $dbname.* TO '$username'@'localhost';"
mysql -u root -p -e "FLUSH PRIVILEGES;"

echo "La base de datos $dbname y el usuario $username se han creado con éxito"



```
