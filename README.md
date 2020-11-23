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

### Usando campos autonuméricos
Los campos autonuméricos, permiten que se vaya sumando +1 por cada nueva fila que vayamos insertando en la tabla.
Para esto, en la consturcción de la tabla, usaremos la característica Autoincrement
```
CREATE TABLE facturas_detalle (
	num_detalle     bigint       not null  AUTO_INCREMENT,
	num_factura     int          not null,
	cantidad        int          not null,
	producto        varchar(10)  not null,
	unitario        double       not null,
	PRIMARY KEY (num_detalle)
);
```
## Operaciones en las tablas de una bases de datos
### Uso de Alias para columnas
Nos permite renombrar temporalmente el nombre de una columna, para la ejecución del código de consulta, utilizando el parametro **AS**
Cuando el nombre que queremos asignar, no usa espacios ni caracteres especiales, se escribe de forma directa.
```
SELECT nombre_cliente AS cliente FROM facturas;
```

Si el nombre que queremos asignar, es compuesto por espacios u otros elementos, es mejor envolverlo con comillas simples
```
SELECT producto AS 'nombre producto' FROM facturas_detalle;
```
### Contar el número de registros de una tabla
La función COUNT de MySQL nos permite contar la cantidad de registros que existen en una tabla.
Es posible combinarlo con la condicional WHERE para obtener resultados diferentes.
Esta funcion, nos entregará un único campo con el resultado.
```
SELECT COUNT(*) FROM facturas_detalle;
```

### Sumar todos los registro de una columna
La función SUM de MySQL nos permite sumar todos los valores de una columna, siempre que ésta se de tipo numérico.
Es posible, según la necesidad, unir la función, con una condicional (clausula WHERE)
Esta funcion, nos entregará un único campo con el resultado.
```
SELECT SUM(unitario) FROM facturas_detalle;
```

## Relaciones, restricciones y claves foraneas
Una clave foranea (también conocida como clave ajena, clave extranjera o clave externa), permite relacionar la tabla
que contiene dicha clave foranea, con otra tabla por medio de su(s) clave(s) primaria(s).
Para poder crear una clave foranea, es necesario formular restricciones mediante cardinalidad:

- 1:1 Permite relacionar 1 registro de la tabla padre con 1 registro de la tabla hijo de manera única e irrepetible.
- 1:N Un registro de la tabla padre, estará presente en varios registros de la tabla hijo.
- N:M Varios registros de la tabla padre, estarán vinculados a varios registros de la tabla hijo.

## Codificación:
Tabla Padre: Creamos una tabla de dos columnas, la primera (pk_col), representará a la clave primaria de la tabla.
```
CREATE TABLE padre (
	pk_col BIGINT UNSIGNED NOT NULL,
	dt_col VARCHAR(40) NOT NULL,
	PRIMARY KEY (pk_col)
);
```

Tabla Hijo: Creamos una tabla de 3 columnas, la primera (id_col), representará a la clave primaria de la tabla, mientras que
pk_col será la clave foranea, relacionada con la tabla padre. Para realizar la relacion, debemos crear un índice (Index), que
por convención, su nombre será compuesto de las siglas "fk" seguido del nombre de la tabla hijo, luego el nombre de la tabla padre
y finalizado con la sigla "idx", cada uno separado por guiones bajos. A pesar de que se le puede dar cualquier nombre, se sugiere
seguir la convención.
También es neceario crear la restricción (Constraint), donde se establece el campo que se usara como clave forenea, la referencia
a la tabla y campo padre y las acciones al borrar el registro y actualizar el registro. Su nombre será escrito, según convensión por la sigla "fk" seguidos de el nombre de la hijo, la tabla padre y separados por guiones bajos. Igual que el índice, puede ser usado cualquier otro nombre, pero se sugiere seguir la convensión.
```
CREATE TABLE hijo (
	id_col BIGINT UNSIGNED NOT NULL,
	pk_col BIGINT UNSIGNED NOT NULL,
	dt_col VARCHAR(40) NOT NULL,
	PRIMARY KEY (id_col),
	INDEX fk_hijo_padre_id (pk_col ASC),
	CONSTRAINT fk_hijo_padre 
		FOREIGN KEY (pk_col)
		REFERENCES padre(pk_col)
		ON DELETE NO ACTION
		ON UPDATE NO ACTION
);
```


