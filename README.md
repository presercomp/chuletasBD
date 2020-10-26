' Conexión al motor MariaDB
mysql -u root
' Listar bases de datos existentes
show databases;
' Usar una base datos
use (nombreBD);
' Crea una base de datos vacia
create database (nombreBD);
' Elimina una base de datos
drop database (nombreBD);
' Cambiar nombre de la bases datos:
'1ero: Respaldar base de datos antigua
mysqldump -u root -p (base antigua) > (nombre_archivo).sql
'2do: acceder a mysql y crear BD nueva
create database (nuevaBD);
'3ro: salir de mysql y volcar archivo antiguo en el nuevo
mysql -u root -p (nuevaBD) < (nombre_archivo)
' Mostrar las tablas existentes en la base de datos seleccionada
show tables;
' Conocer la estructura de una tabla
describe (nombreTabla);
' Eliminar una tabla
drop table (nombreTabla);
' Conocer el contenido de una tabla.
SELECT * FROM (nombreTabla)