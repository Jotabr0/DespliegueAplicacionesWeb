```

#!/bin/bash



# Obtener el nombre del subdominio
read -p "Introduce el nombre del subdominio: " subdomain

# Obtener la dirección IP del servidor
ip=$(hostname -I | awk '{print $1}')

# Crear el archivo de zona directa
cat >> /etc/bind/named.conf.local <<EOF

zone "$subdomain.example.com" {
  type master;
  file "/etc/bind/db.$subdomain.example.com";
};
EOF

# Crear el archivo de zona directa
cat > /etc/bind/db.$subdomain.example.com <<EOF
\$TTL    604800
@       IN      SOA     ns.example.com. root.example.com. (
                  1         ; Serial
             604800         ; Refresh
              86400         ; Retry
            2419200         ; Expire
             604800 )       ; Negative Cache TTL
;
@       IN      NS      ns.example.com.
@       IN      A       $ip
EOF

# Crear el archivo de zona inversa
cat >> /etc/bind/named.conf.local <<EOF

zone "$(echo $ip | awk -F '.' '{print $3"."$2"."$1".in-addr.arpa"}')" {
  type master;
  file "/etc/bind/db.$(echo $ip | awk -F '.' '{print $3"."$2"."$1".in-addr.arpa"}')";
};
EOF

# Crear el archivo de zona inversa
cat > /etc/bind/db.$(echo $ip | awk -F '.' '{print $3"."$2"."$1".in-addr.arpa"}') <<EOF
\$TTL    604800
@       IN      SOA     ns.example.com. root.example.com. (
                  1         ; Serial
             604800         ; Refresh
              86400         ; Retry
            2419200         ; Expire
             604800 )       ; Negative Cache TTL
;
$(echo $ip | awk -F '.' '{print $4}')     IN      PTR     $subdomain.example.com.
EOF

# Reiniciar el servicio de BIND
systemctl restart bind9

echo "El subdominio $subdomain.example.com se ha creado con éxito"




```
