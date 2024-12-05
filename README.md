# Crear el usuario
CREATE USER zabbix WITH LOGIN ENCRYPTED PASSWORD 'zabbix' CREATEDB;

# Levantar el contenedor
docker compose up -d

# Ejecutar manualmente para obtener el script de base de datos a ser ejecutado posteriormente en postgresql
docker exec -it zabbix-server sh -c "gzip -dc /usr/share/doc/zabbix-server-postgresql/create.sql.gz" > create.sql

# ejecutar el script en el servidor de base de datos
sed -i 's/\x9d//g' create.sql
psql -h 192.168.6.114 -p 5434 -U zabbix -d zabbix -f create.sql

# Volver a levantar zabbix
docker-compose down
docker-compose up -d
