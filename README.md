
-- Creación de tablas de la base de datos compra_venta_autos

-- Creación de la ddbb
CREATE DATABASE COMPRA_VENTA_AUTOS; 
USE COMPRA_VENTA_AUTOS;

-- Creación de la tabla clientes (datos personales de las personas que nos venden sus autos)
	CREATE TABLE COMPRA_VENTA_AUTOS.CLIENTES( 
	ID_CLIENTE	INT NOT NULL AUTO_INCREMENT 
	,NOMBRE	VARCHAR(20) NOT NULL
	,APELLIDO	VARCHAR(20) NOT NULL
	,DNI	INT NOT NULL
	,DIRECCION	VARCHAR(50) NOT NULL
	,CP	VARCHAR(10) NOT NULL
	,MAIL	VARCHAR(50) NOT NULL
	,PRIMARY KEY (ID_CLIENTE)
	);
    
-- Creación de la tabla autos (registro de autos que compramos)
    CREATE TABLE COMPRA_VENTA_AUTOS.AUTOS(
	ID_AUTO	INT NOT NULL AUTO_INCREMENT
	,PATENTE	VARCHAR(7) NOT NULL
	,ID_MARCA_MODELO	INT NOT NULL
	,ANO INT NOT NULL
	,TRANSMISION VARCHAR(20) NOT NULL
    ,COLOR VARCHAR(20)
	,PRIMARY KEY (ID_AUTO)
	);
    
-- Creación de la tabla marca_modelo (marcas y modelos de autos que compramos)
        CREATE TABLE COMPRA_VENTA_AUTOS.MARCA_MODELO(
	ID_MARCA_MODELO	INT NOT NULL 
	,MARCA VARCHAR(20) NOT NULL
	,MODELO VARCHAR(20) NOT NULL
	,PRIMARY KEY (ID_MARCA_MODELO)


	);
    
-- Creación de la tabla telefonos_clientes (registro de los telefonos de las personas que nos venden sus autos)
	CREATE TABLE COMPRA_VENTA_AUTOS.TELEFONOS_CLIENTES(
    ID_CLIENTE	INT NOT NULL
	,TELEFONO_PRINCIPAL BIGINT NOT NULL
    ,TELEFONO_SECUNDARIO BIGINT
	,PRIMARY KEY (ID_CLIENTE)
	,FOREIGN KEY (ID_CLIENTE) REFERENCES COMPRA_VENTA_AUTOS.CLIENTES(ID_CLIENTE)
	);
    
-- Creación de la tabla status_ots (status posibles de las ordenes de trabajo)
	CREATE TABLE COMPRA_VENTA_AUTOS.STATUS_OTS(
	ID_STATUS	INT  NOT NULL  AUTO_INCREMENT
	,ESTATUS	VARCHAR(50)  NOT NULL
	,DESCRIPCION_STATUS	VARCHAR(100)
	,PRIMARY KEY (ID_STATUS)
    );
    
-- Creación de la tabla mecanicos (datos personales de los mecanicos que trabajan en el taller)
	CREATE TABLE COMPRA_VENTA_AUTOS.MECANICOS(
	ID_MECANICO	INT  NOT NULL  AUTO_INCREMENT
	,NOMBRE	VARCHAR(20) NOT NULL
	,APELLIDO	VARCHAR(20) NOT NULL
	,DNI	INT NOT NULL
	,LEGAJO	INT NOT NULL
	,TELEFONO BIGINT NOT NULL
	,DIRECCION	VARCHAR(50) NOT NULL
	,CP	VARCHAR(10) NOT NULL
	,MAIL	VARCHAR(50) NOT NULL
	,PRIMARY KEY (ID_MECANICO)
    );

-- Creación de la tabla sector (sectores dentro del taller)
	CREATE TABLE COMPRA_VENTA_AUTOS.SECTOR(
	ID_SECTOR	INT  NOT NULL  AUTO_INCREMENT
	,SECTOR	VARCHAR(100)  NOT NULL
	,DESCRIPCION	VARCHAR(100) 
	,PRIMARY KEY (ID_SECTOR)
    );
    
-- Creación de la tabla polivalencia (sectores en los que trabajan los mecanicos)
	CREATE TABLE COMPRA_VENTA_AUTOS.POLIVALENCIA(
	ID_POLIVALENCIA INT  NOT NULL  AUTO_INCREMENT
    ,ID_MECANICO	INT  NOT NULL
	,ID_SECTOR	INT NOT NULL
	,PRIMARY KEY (ID_POLIVALENCIA)
	,FOREIGN KEY (ID_MECANICO) REFERENCES COMPRA_VENTA_AUTOS.MECANICOS(ID_MECANICO)
    ,FOREIGN KEY (ID_SECTOR) REFERENCES COMPRA_VENTA_AUTOS.SECTOR(ID_SECTOR)
    );
    
-- Creación de la tabla categorias (categorias dentro de los sectores)
    CREATE TABLE COMPRA_VENTA_AUTOS.CATEGORIAS(
	ID_CATEGORIA INT  NOT NULL  AUTO_INCREMENT
	,NOMBRE	VARCHAR(50)  NOT NULL
	,ID_SECTOR INT NOT NULL
    ,PRIMARY KEY (ID_CATEGORIA)
	,FOREIGN KEY (ID_SECTOR) REFERENCES COMPRA_VENTA_AUTOS.SECTOR(ID_SECTOR)
    );
 
-- Creación de la tabla grupos (grupos de trabajo dentro de las categorias)
    CREATE TABLE COMPRA_VENTA_AUTOS.GRUPO(
	ID_GRUPO_TRABAJO	INT  NOT NULL  AUTO_INCREMENT
	,ID_CATEGORIA	INT  NOT NULL
	,NOMBRE	VARCHAR(50)  NOT NULL
	,PRIMARY KEY (ID_GRUPO_TRABAJO)
    );

-- Creación de la tabla compras (registro de las compras que se realizan como evento)
    create TABLE COMPRA_VENTA_AUTOS.COMPRAS(
	ID_COMPRA INT NOT NULL AUTO_INCREMENT 
	,ID_CLIENTE INT NOT NULL
	,ID_AUTO	INT NOT NULL
    ,FECHA_CONTRATO DATETIME NOT NULL
    ,FECHA_ALTA_SISTEMA DATETIME NOT NULL
    ,MONTO VARCHAR(20) NOT NULL
	,PRIMARY KEY (ID_COMPRA)
	,FOREIGN KEY (ID_AUTO) REFERENCES COMPRA_VENTA_AUTOS.AUTOS(ID_AUTO)
	);
	 
-- Creación de la tabla autos (registro de las ordenes de trabajo a realizar)
	CREATE TABLE COMPRA_VENTA_AUTOS.OTS(
	ID_AUTO	INT NOT NULL
	,ID_OT	INT NOT NULL AUTO_INCREMENT
	,DESCRIPCION_OT	VARCHAR(100)
	,ID_SECTOR INT NOT NULL
    ,ID_CATEGORIA INT NOT NULL
    ,ID_GRUPO_TRABAJO INT NOT NULL
    ,ID_STATUS_OT INT NOT NULL
	,PRIMARY KEY (ID_OT)
	,FOREIGN KEY (ID_AUTO) REFERENCES COMPRA_VENTA_AUTOS.AUTOS(ID_AUTO)
    ,FOREIGN KEY (ID_GRUPO_TRABAJO) REFERENCES COMPRA_VENTA_AUTOS.GRUPO(ID_GRUPO_TRABAJO)
    ,FOREIGN KEY (ID_CATEGORIA) REFERENCES COMPRA_VENTA_AUTOS.CATEGORIAS(ID_CATEGORIA)
    ,FOREIGN KEY (ID_STATUS_OT) REFERENCES COMPRA_VENTA_AUTOS.STATUS_OTS(ID_STATUS)
    ,FOREIGN KEY (ID_SECTOR) REFERENCES COMPRA_VENTA_AUTOS.SECTOR(ID_SECTOR)
    );
    
-- Creación de la tabla ejecucion_ots (registro de que mecanico ejecutó la orden de trabajo)
	CREATE TABLE COMPRA_VENTA_AUTOS.EJECUCION_OTS(
	ID_EJECUCION INT NOT NULL AUTO_INCREMENT
    ,ID_OT INT NOT NULL
	,ID_MECANICO INT NOT NULL
    ,ID_SECTOR INT NOT NULL
	,PRIMARY KEY (ID_EJECUCION)
	,FOREIGN KEY (ID_OT) REFERENCES COMPRA_VENTA_AUTOS.OTS(ID_OT)
    ,FOREIGN KEY (ID_MECANICO) REFERENCES COMPRA_VENTA_AUTOS.MECANICOS(ID_MECANICO)
    ,FOREIGN KEY (ID_sector) REFERENCES COMPRA_VENTA_AUTOS.sector(ID_sector)
    );

-- Creación de la tabla de tipo log donde se van a guardar los cambios realizados en la tabla compras
create table log_compra (
    id_compra int,
    changes varchar(250), 
    date timestamp,
    time timestamp,
    USER_LOG VARCHAR(50)
);

-- Creación de la tabla de tipo log donde se van a guardar los cambios realizados en la tabla clientes
CREATE table log_clientes (
    id_log_clientes int auto_increment primary key,
    action varchar(30), 
    id_cliente int, 
    changes varchar(500),
    date timestamp,
    time timestamp,
    USER_LOG VARCHAR(50)
);

-- Creación de vistas

-- Creación de vista que muestra un join entre la tabla autos y la tabla marca_modelo
CREATE VIEW INFO_AUTOS AS 
(SELECT PATENTE,ANO,TRANSMISION,COLOR,MARCA,MODELO
FROM COMPRA_VENTA_AUTOS.AUTOS AUTOS
INNER JOIN COMPRA_VENTA_AUTOS.MARCA_MODELO MM
ON AUTOS.ID_MARCA_MODELO = MM.ID_MARCA_MODELO);

-- Creación de vista que muestra la cantidad de autos comprados del año 2010
CREATE VIEW CANT_AUTOS_2010 AS
(SELECT COUNT(*) AS CANT_AUTOS_2010
FROM COMPRA_VENTA_AUTOS.AUTOS 
WHERE ANO = 2010);

-- Creación de vista que muestra la cantidad de mecanicos que trabajan en cada sector
CREATE VIEW MECANICOS_SECTOR AS
(SELECT ID_SECTOR AS SECTOR,
COUNT(*) AS MECANICOS_POR_SECTOR
FROM POLIVALENCIA
GROUP BY ID_SECTOR);

-- Creación de vista que muestra las ordenes de trabajo aprobadas
CREATE VIEW OTS_APROBADAS AS
(SELECT SECTOR, COUNT(ID_OT) AS CANT_OTS
FROM COMPRA_VENTA_AUTOS.OTS OTS
INNER JOIN COMPRA_VENTA_AUTOS.STATUS_OTS STATUS
ON OTS.ID_STATUS_OT = STATUS.ID_STATUS
INNER JOIN COMPRA_VENTA_AUTOS.SECTOR SECTOR
ON OTS.ID_SECTOR = SECTOR.ID_SECTOR
WHERE ESTATUS = 'Aprobado'
GROUP BY SECTOR
);

-- Creación de vista que muestra la cantidad de firmas de contrato por fecha
CREATE VIEW CANT_CONTRATOS_FECHA AS
(SELECT FECHA_CONTRATO AS FECHA, COUNT(*) AS CANT_FECHA
FROM COMPRAS
GROUP BY FECHA_CONTRATO);
	
-- Script creación de funciones

-- Creación de función que trae la patente cuando se ingresa el id_auto
delimiter ##
create function get_car(carid int)
returns varchar(7)
reads sql data

begin

 declare plate varchar(7);
 select patente into plate from autos where id_auto = carid;
 
 return plate;
 
end##

delimiter ;

select get_car(1)

-- Creación de función que trae el nombre y apellido del mecanico concatenado cuando ingresas id_mecanico
delimiter ##
create function get_mecanic(mecanicid int)
returns varchar(40)
reads sql data

begin

 declare name varchar(40);
 select concat(nombre,' ',apellido) into name from mecanicos where id_mecanico = mecanicid;
 
 return name;
 
end##

delimiter ;

-- Visualizamos
select get_mecanic(1)

-- Creación de función que me cuente la cantidad de ordenes de trabajo ejecutadas por mecanico y que en función de eso los califique
delimiter ##
create function nivel_mecanico (id_mecanico int)
returns varchar(20)
reads sql data
begin

declare cant_ots varchar(20);
declare q_ots int;

select count(*) into q_ots from ejecucion_ots e where e.id_mecanico = id_mecanico;

if q_ots >=10 then
	set cant_ots = "high performance";
elseif q_ots between 6 and 10 then
	set cant_ots = "achiever";
elseif q_ots between 4 and 5 then
	set cant_ots = "below";
else
	set cant_ots = "issue";
end if;

return(cant_ots);

end##
delimiter ;

-- Visualizamos
select id_mecanico, nivel_mecanico(id_mecanico)
from ejecucion_ots

-- Creación de stored procedures

-- Creación de S.P. que permite indicar a través de un parámetro el campo de ordenamiento de una tabla y mediante un segundo parámetro, si el orden es descendente o ascendente.
use compra_venta_autos
DELIMITER $$
create PROCEDURE cars_ordered(column1 varchar(100), orden int) -- declaro los parametros de entrada (column1 es el campo que va a indicar el ordenamiento y orden asc o desc)
BEGIN  
        declare query_base varchar(250);
        declare tipo_orden varchar(10); 
        declare query_final varchar(250);
        
        set query_base = ' select patente,color from compra_venta_autos.autos ';
        
        -- abro if para indicar el tipo de ordenamiento
        
        if orden = 1 then 
            set tipo_orden = ' asc;';
        else 
            set tipo_orden = ' desc;';
        end if;
        
        if column1 = '' then
            select 'La columna no puede ser vacia';
        else 
            select concat(query_base, ' order by ', column1, ' ', tipo_orden) into query_final; -- resultado final segun los parametros de entrada
            
            SET @smtmt = query_final;
            
            PREPARE EJECUTAR FROM @smtmt;
            EXECUTE EJECUTAR;
            deallocate prepare EJECUTAR;
    END IF;
    
END$$
DELIMITER ;

-- Para visualizar
CALL cars_ordered('', 1);

-- Creación de S.P. que puede insertar registros en una tabla
-- Primero creo un S.P que traiga el ultimo cliente (nos va a servir para después generar un cliente mayor al mismo)
USE COMPRA_VENTA_AUTOS
DELIMITER $$
CREATE PROCEDURE ULTIMO_CLIENTE(OUT nUser int)
BEGIN                    

    SELECT max(ID_CLIENTE) into nUser FROM clientes;

END$$

DELIMITER ;

-- Ahora si creo un S.P para la generacion del cliente
use COMPRA_VENTA_AUTOS;
DELIMITER $$
CREATE PROCEDURE GENERA_CLIENTE(nombre varchar(100), apellido varchar(100), dni varchar(100), direccion varchar(100), cp varchar(10), mail varchar(50))
BEGIN                    
    DECLARE ID INT;                                  
    CALL ULTIMO_CLIENTE(ID);            
    SET ID = ID +1;  -- Genero un id_cliente mayor al ultimo                                                     
    
    insert into clientes values (id, nombre, apellido, dni, direccion, cp, mail);

END$$

DELIMITER ;

CALL GENERA_CLIENTE('RAQUEL','BARRETO','39913875','TUCUMAN 2062','1051','raquel.barreto@gmail.com');

-- Para visualizar
select *
from CLIENTES
order by ID_CLIENTE desc





    

