# Lista de Comandos utilizados en MySQL / MariaDB

## Conexión al motor MariaDB
| parametro  |  Accion   |
|--------|-------|
| -u     | Indica que requiere un usuario |
| -p     | Indica que requiere una clave  |


### Comandos:
```
mysql -u root
```
```
mysql -u root -p
```

## Listar bases de datos existentes
```
show databases;
```

## Usar una base datos
```
use [nombreBD];
```

## Crea una base de datos vacia
```
create database [nombreBD];
```

## Elimina una base de datos
```
drop database [nombreBD];
```

## Cambiar nombre de la bases datos:
1. Respaldar base de datos antigua
2. Acceder al motor
3. Crear BD nueva
4. Salir del motor y cargar respaldo en BD nueva

```
mysqldump -u root -p [base antigua] > [nombre_archivo].sql
```
```
mysql -u root
```
```
create database [nuevaBD];
```
```
mysql -u root -p [nuevaBD] < [nombre_archivo]
```

## Mostrar las tablas existentes en la base de datos seleccionada
```
show tables;
```

## Conocer la estructura de una tabla
```
describe [nombreTabla];
```

## Eliminar una tabla
```
drop table [nombreTabla];
```
## Conocer el contenido de una tabla.
```
SELECT * FROM [nombreTabla]
```

## Creación de tablas
Estructura:

| Descripcion | Nombre Columna| Tipo de dato |Obligatorio |
| ---- | ---- | ---- | ---- |
| Numero Factura | num_factura | INT | Obligatorio |
| Fecha emisión | fecha_emision | DATE  | Opcional |
| Rut del cliente  | rut_cliente | INT(9) / VARCHAR(11) | Obligatorio |
| Nombre del cliente | nombre_cliente | VARCHAR(100) | Obligatorio |
| Monto | monto | DOUBLE | Obligatorio |

### Tabla con todos los campos opcionales
```
CREATE TABLE facturas (
	num_factura     int,
	fecha_emision   date,
	rut_cliente     varchar(11),
	nombre_cliente  varchar(100),
	monto           double
);
```
### Tabla con el esquema de restricciones obligatorios
```
CREATE TABLE facturas (
	num_factura     int          not null,
	fecha_emision   date         null       default CURRENT_TIMESTAMP,
	rut_cliente     varchar(11)  not null,
	nombre_cliente  varchar(100) not null,
	monto           double       not null,
	PRIMARY KEY (num_factura)
);
```

## Despliegue de datos

### Mostrar todas las columnas de la tabla
```
SELECT * FROM facturas;
```
### Mostrar algunas columnas de la tabla
```
SELECT num_factura, nombre_cliente, monto FROM facturas;
```
### Mostrar algunos datos de la tabla usando filtros
```
SELECT * FROM facturas WHERE [campo][Operador comparación][valor]
```

**Observaciones:** 
1. Deberá usar elementos de comparación según tipo de datos

| Operador | Descripcion | Tipo de dato aplicable |
| ---- | ---- | ---- | 
| =  | Compara que dos elementos sean iguales | Todos | 
| >  | Compara si el primer elemento es mayor que el segundo | Numéricos |
| >= | Compara si el primer elemento es mayor o igual que el segundo | Numéricos |
| <  | Compara si el primer elemento es menor que el segundo | Numéricos |
| <= | Compara si el primer elemento es menor o igual que el segundo | Numéricos |
| != | Compara que dos elementos sean iguales | Todos |

2. Los valores para campos de tipo CHAR, VARCHAR, DATE siempre debe ir entre comillas simples ' '
```
SELECT * FROM facturas WHERE rut = '20904725-K';
```

3. Si se desea filtrar por 2 o más columnas, deberá usar los operadores de anidacion (AND / OR) 


## Inserción de datos

### Insertar datos en todas las columnas, sin definirlas
```
INSERT INTO facturas VALUES 
(1, '2020-10-01', '20904725-K', 'Israel Parra Berna', 159900.0);
```

###  Insertar datos declarando columnas.
```
INSERT INTO facturas (num_factura, monto, rut_cliente, nombre_cliente) VALUES 
(2, 69870.0, '20025724-3', 'Cristian Gonzalez Codoceo');
```

###  Insertar varios datos en una sola consulta:
(observación: se puede usar el inicio de las opciones 1 o 2)
```
INSERT INTO facturas VALUES 
(3, '2020-10-03', '20.718.765-8', 'Yosthyn Jose Gutierrez Navarro', 57890.0),
(4, '2020-10-04', '16.388.841-6', 'Karina Andrea Vilches Carvajal', 309700.0),
(5, '2020-10-05', '20.406.677-9', 'Katherine Elizabeth Vega Carrasco', 43790.0),
(6, '2020-10-06', '20.718.737-2', 'Anastassia Belen Araya Rojas', 295990.0);
```

## Edición de datos

> **Advertencia:** El uso inadecuado de la condición where, puede provocar resultados indeseados.

### Mala practica:
```
UPDATE facturas SET rut_cliente = '20718765-8';
```
### Buena practica:
```
UPDATE facturas SET rut_cliente = '20904725-K' WHERE num_factura = 1;
```

## Eliminación de datos

> **Advertencia:** El uso inadecuado de la condición where, puede provocar resultados indeseados.


### Mala practica:
```
DELETE FROM facturas;
```
### Buena practica:
```
DELETE FROM facturas WHERE num_factura = 1;
```