```
sudo apt-get update
sudo apt-get install libapache2-mod-wsgi-py3

```



```

#!/bin/bash



# Crear el archivo wsgi.py
echo -e "def application(environ,start_response):\n    status = '200 OK'\n    html = 'html>\n' \\\n           '<body>\n' \\\n           '<div style=\"width: 100%; font-size: 40px; font-weight: bold; text-align: center;\">\n' \\\n           'Welcome to mod_wsgi Test Page\n' \\\n           '</div>\n' \\\n           '</body>\n' \\\n           '</html>\n'\n    response_header = [('Content-type','text/html')]\n    start_response(status,response_header)\n    return [html]" > /var/www/html/wsgi.py

# Cambiar la propiedad del archivo a www-data
chown www-data:www-data /var/www/html/wsgi.py

# Crear el archivo de configuración del host virtual de Apache
echo "WSGIScriptAlias /wsgi /var/www/html/wsgi.py" > /etc/apache2/conf-available/wsgi.conf

# Activar la configuración de mod-wsgi
a2enconf wsgi

# Reiniciar el servicio de Apache
systemctl restart apache2




```
