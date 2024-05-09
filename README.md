# BASE DE DATOS GARDEN 

### DER

![DER_Garden_DB](https://github.com/Fauroro/garden_DB/blob/main/DER_GardenDB.png?raw=true)



## Creación de la DB y sus Tablas

```mysql
-- -----------------------------------------------------
-- Database garden_DB
-- -----------------------------------------------------
CREATE DATABASE IF NOT EXISTS garden_DB;
USE garden_DB;

-- -----------------------------------------------------
-- Table gama_producto
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS gama_producto (
  codigo_gama INT(11) NOT NULL,
  gama VARCHAR(50) NOT NULL,
  descripcion_texto TEXT(50) NULL,
  descripcion_html TEXT(50) NULL,
  imagen VARCHAR(256) NULL,
  PRIMARY KEY (codigo_gama)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table dimensiones
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS dimensiones (
  codigo_dimensiones VARCHAR(15) NOT NULL,
  largo VARCHAR(7) NULL,
  alto VARCHAR(7) NULL,
  ancho VARCHAR(7) NULL,
  peso VARCHAR(7) NULL,
  PRIMARY KEY (codigo_dimensiones)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table producto
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS producto (
  codigo_producto VARCHAR(15) NOT NULL,
  nombre VARCHAR(70) NOT NULL,
  codigo_gama INT(11) NOT NULL,
  cantidad_stock SMALLINT(6) NOT NULL,
  precio_venta DECIMAL(15,2) NOT NULL,
  descripcion TEXT NULL,
  codigo_dimensiones VARCHAR(15) NOT NULL,
  PRIMARY KEY (codigo_producto),
  CONSTRAINT codigo_gama FOREIGN KEY (codigo_gama) REFERENCES gama_producto (codigo_gama),
  CONSTRAINT codigo_dimensiones FOREIGN KEY (codigo_dimensiones) REFERENCES dimensiones (codigo_dimensiones)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table proveedor
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS proveedor (
  codigo_proveedor INT(11) NOT NULL,
  nombre VARCHAR(50) NOT NULL,
  email VARCHAR(60) NOT NULL,
  PRIMARY KEY (codigo_proveedor)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table producto_proveedor
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS producto_proveedor (
  codigo_producto VARCHAR(15) NOT NULL,
  codigo_proveedor INT(11) NOT NULL,
  precio_proveedor DECIMAL(15,2) NOT NULL,
  PRIMARY KEY (codigo_producto, codigo_proveedor),
  CONSTRAINT FK_cod_producto FOREIGN KEY (codigo_producto) REFERENCES producto (codigo_producto),
  CONSTRAINT FK_cod_proveedor FOREIGN KEY (codigo_proveedor) REFERENCES proveedor (codigo_proveedor)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table estado
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS estado (
  codigo_estado INT NOT NULL AUTO_INCREMENT,
  nombre VARCHAR(25) NOT NULL,
  PRIMARY KEY (codigo_estado))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table pais
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS pais (
  codigo_pais VARCHAR(5) NOT NULL,
  nombre_pais VARCHAR(50) NOT NULL,
  PRIMARY KEY (codigo_pais))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table region
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS region (
  codigo_region VARCHAR(10) NOT NULL,
  nombre_region VARCHAR(50) NOT NULL,
  codigo_pais VARCHAR(5) NOT NULL,
  PRIMARY KEY (codigo_region),
  CONSTRAINT codigo_pais FOREIGN KEY (codigo_pais) REFERENCES pais (codigo_pais)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table ciudad
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS ciudad (
  codigo_ciudad INT(11) NOT NULL,
  nombre_ciudad VARCHAR(50) NOT NULL,
  codigo_region VARCHAR(10) NOT NULL,
  PRIMARY KEY (codigo_ciudad),
  CONSTRAINT codigo_region FOREIGN KEY (codigo_region) REFERENCES region (codigo_region)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table oficina
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS oficina (
  codigo_oficina INT(11) NOT NULL,
  codigo_ciudad INT(11) NOT NULL,
  codigo_postal VARCHAR(10) NOT NULL,
  PRIMARY KEY (codigo_oficina),
  CONSTRAINT FK_ofi_ciudad FOREIGN KEY (codigo_ciudad) REFERENCES ciudad (codigo_ciudad)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table puesto_empleado
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS puesto_empleado (
  codigo_puesto_empleado INT(11) NOT NULL,
  nombre_puesto VARCHAR(45) NOT NULL,
  extension VARCHAR(10) NOT NULL,
  PRIMARY KEY (codigo_puesto_empleado)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table empleado
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS empleado (
  codigo_empleado INT(11) NOT NULL AUTO_INCREMENT,
  nombre_empleado VARCHAR(50) NOT NULL,
  apellido1_empleado VARCHAR(50) NOT NULL,
  apellido2_empleado VARCHAR(50) NULL,
  email_empleado VARCHAR(100) NOT NULL,
  codigo_oficina INT(11) NOT NULL,
  codigo_jefe INT(11) NULL,
  codigo_puesto_empleado INT(11) NOT NULL,
  PRIMARY KEY (codigo_empleado),
  CONSTRAINT FK_emple_oficina FOREIGN KEY (codigo_oficina) REFERENCES oficina (codigo_oficina),
  CONSTRAINT FK_cod_jefe FOREIGN KEY (codigo_jefe) REFERENCES empleado (codigo_empleado),
  CONSTRAINT FK_puesto_empleado FOREIGN KEY (codigo_puesto_empleado) REFERENCES puesto_empleado (codigo_puesto_empleado)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table cliente
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS cliente (
  codigo_cliente INT(11) NOT NULL,
  nombre_cliente VARCHAR(50) NOT NULL,
  codigo_ciudad INT(11) NOT NULL,
  codigo_postal VARCHAR(10) NOT NULL,
  limite_credito DECIMAL(15,2) NULL,
  codigo_rep_ventas INT(11) NOT NULL,
  PRIMARY KEY (codigo_cliente),
  CONSTRAINT FK_cliente_ciudad FOREIGN KEY (codigo_ciudad) REFERENCES ciudad (codigo_ciudad),
  CONSTRAINT FK_cod_rep_ventas FOREIGN KEY (codigo_rep_ventas) REFERENCES empleado (codigo_empleado)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table pedido
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS pedido (
  codigo_pedido VARCHAR(15) NOT NULL,
  fecha_pedido DATE NOT NULL,
  fecha_esperada DATE NOT NULL,
  fecha_entrega DATE NULL,
  comentarios TEXT NULL,
  codigo_cliente INT(11) NOT NULL,
  codigo_estado INT NOT NULL,
  PRIMARY KEY (codigo_pedido),
  CONSTRAINT FK_pedido_estado FOREIGN KEY (codigo_estado) REFERENCES estado (codigo_estado),
  CONSTRAINT FK_pedido_cliente FOREIGN KEY (codigo_cliente) REFERENCES cliente (codigo_cliente)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table detalle_pedido
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS detalle_pedido (
  codigo_producto VARCHAR(15) NOT NULL,
  codigo_pedido VARCHAR(15) NOT NULL,
  cantidad INT(11) NOT NULL,
  precio_unidad DECIMAL(15,2) NOT NULL,
  numero_linea SMALLINT(6) NOT NULL,
  PRIMARY KEY (codigo_producto, codigo_pedido),
  CONSTRAINT FK_det_ped_prod FOREIGN KEY (codigo_producto) REFERENCES producto (codigo_producto),
  CONSTRAINT FK_codigo_pedido FOREIGN KEY (codigo_pedido) REFERENCES pedido (codigo_pedido)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table tipo_telefono
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS tipo_telefono (
  codigo_tipo_telefono INT(11) NOT NULL AUTO_INCREMENT,
  nombre_tipo_telefono VARCHAR(50) NOT NULL,
  PRIMARY KEY (codigo_tipo_telefono)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table telefono_cliente
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS telefono_cliente (
  codigo_telefono_cliente INT(11) NOT NULL AUTO_INCREMENT,
  telefono_cliente VARCHAR(50) NOT NULL,
  codigo_cliente INT(11) NOT NULL,
  codigo_tipo_telefono INT(11) NULL,
  PRIMARY KEY (codigo_telefono_cliente),
  CONSTRAINT FK_tipo_tel FOREIGN KEY (codigo_tipo_telefono) REFERENCES tipo_telefono (codigo_tipo_telefono),
  CONSTRAINT FK_cliente_tel FOREIGN KEY (codigo_cliente) REFERENCES cliente (codigo_cliente)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table tipo_direccion
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS tipo_direccion (
  codigo_tipo_dir SMALLINT NOT NULL AUTO_INCREMENT,
  nombre_tipo_dir VARCHAR(50) NOT NULL,
  PRIMARY KEY (codigo_tipo_dir)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table direccion cliente
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS direccion (
  codigo_direccion INT(11) NOT NULL AUTO_INCREMENT,
  nombre_calle VARCHAR(50) NULL,
  numero_direccion VARCHAR(50) NULL,
  nombre_barrio VARCHAR(50) NULL,
  codigo_cliente INT(11) NOT NULL,
  codigo_tipo_dir SMALLINT NOT NULL,
  PRIMARY KEY (codigo_direccion),
  CONSTRAINT FK_cliente_dir FOREIGN KEY (codigo_cliente) REFERENCES cliente (codigo_cliente),
  CONSTRAINT FK_dir_tipo_dir FOREIGN KEY (codigo_tipo_dir) REFERENCES tipo_direccion (codigo_tipo_dir)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table contacto
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS contacto (
  codigo_contacto VARCHAR(25) NOT NULL,
  primer_nombre_contacto VARCHAR(50) NOT NULL,
  primer_apellido_contacto VARCHAR(50) NOT NULL,
  email_contacto VARCHAR(50) NOT NULL,
  segundo_apellido_contacto VARCHAR(50) NULL,
  segundo_nombre_contacto VARCHAR(50) NULL,
  codigo_cliente INT(11) NOT NULL,
  PRIMARY KEY (codigo_contacto),
  CONSTRAINT FK_contact_cliente FOREIGN KEY (codigo_cliente) REFERENCES cliente (codigo_cliente)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table metodo_pago
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS metodo_pago (
  codigo_metodo_pago INT NOT NULL AUTO_INCREMENT,
  nombre_met_pago VARCHAR(50) NOT NULL,
  PRIMARY KEY (codigo_metodo_pago)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table pago
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS pago (
  codigo_pago INT(11) NOT NULL AUTO_INCREMENT,
  fecha_pago DATE NOT NULL,
  total_pago DECIMAL(15,2) NOT NULL,
  codigo_cliente INT(11) NOT NULL,
  codigo_met_pago INT(11) NOT NULL,
  PRIMARY KEY (codigo_pago),
  CONSTRAINT FK_pago_cliente FOREIGN KEY (codigo_cliente) REFERENCES cliente (codigo_cliente),
  CONSTRAINT FK_cod_met_pago FOREIGN KEY (codigo_met_pago) REFERENCES metodo_pago (codigo_metodo_pago)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table direccion_oficina
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS direccion_oficina (
  codigo_direccion INT(11) NOT NULL AUTO_INCREMENT,
  nombre_calle VARCHAR(50) NULL,
  numero_direccion VARCHAR(50) NULL,
  nombre_barrio VARCHAR(50) NULL,
  codigo_oficina INT(10) NOT NULL,
  codigo_tipo_dir SMALLINT NOT NULL,
  PRIMARY KEY (codigo_direccion),
  CONSTRAINT FK_dir_oficina FOREIGN KEY (codigo_oficina) REFERENCES oficina (codigo_oficina),
  CONSTRAINT FK_tipo_dir_ofi FOREIGN KEY (codigo_tipo_dir) REFERENCES tipo_direccion (codigo_tipo_dir)
)ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table telefono_oficina
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS telefono_oficina (
  codigo_telefono_oficina INT(11) NOT NULL AUTO_INCREMENT,
  telefono_oficina VARCHAR(50) NOT NULL,
  codigo_tipo_telefono INT(11) NOT NULL,
  codigo_oficina INT(11) NOT NULL,
  PRIMARY KEY (codigo_telefono_oficina),
  CONSTRAINT FK_tipo_tel_ofi FOREIGN KEY (codigo_tipo_telefono) REFERENCES tipo_telefono (codigo_tipo_telefono),
  CONSTRAINT FK_tel_oficina FOREIGN KEY (codigo_oficina) REFERENCES oficina (codigo_oficina)
)ENGINE = InnoDB;

```



### Inserción de datos

```mysql
-- -----------------------------------------------------
-- INSERT gama_producto
-- ----------------------------------------------------
INSERT INTO gama_producto VALUES (1,'Herbaceas','Plantas para jardin decorativas',NULL,NULL);
INSERT INTO gama_producto VALUES (2,'Herramientas','Herramientas para todo tipo de acción',NULL,NULL);
INSERT INTO gama_producto VALUES (3,'Aromáticas','Plantas aromáticas',NULL,NULL);
INSERT INTO gama_producto VALUES (4,'Frutales','Árboles pequeños de producción frutal',NULL,NULL);
INSERT INTO gama_producto VALUES (5,'Ornamentales','Plantas vistosas para la decoración del jardín',NULL,NULL);


-- ----------------------------------------------------
-- INSERT DIMENSIONES
-- ----------------------------------------------------
INSERT INTO dimensiones VALUES ('DIM001', '100', '50', '30', '10');
INSERT INTO dimensiones VALUES ('DIM002', '80', '40', '25', '8');
INSERT INTO dimensiones VALUES ('DIM003', '120', '60', '35', '12');
INSERT INTO dimensiones VALUES ('DIM004', '90', '45', '28', '9');
INSERT INTO dimensiones VALUES ('DIM005', '110', '55', '32', '11');
INSERT INTO dimensiones VALUES ('DIM006', '95', '47', '29', '9.5');
INSERT INTO dimensiones VALUES ('DIM007', '105', '52', '31', '10.5');
INSERT INTO dimensiones VALUES ('DIM008', '85', '42', '26', '8.5');
INSERT INTO dimensiones VALUES ('DIM009', '115', '57', '33', '11.5');
INSERT INTO dimensiones VALUES ('DIM010', '125', '62', '36', '12.5');
INSERT INTO dimensiones VALUES ('DIM011', '130', '65', '38', '13');
INSERT INTO dimensiones VALUES ('DIM012', '75', '37', '23', '7.5');
INSERT INTO dimensiones VALUES ('DIM013', '140', '70', '40', '14');
INSERT INTO dimensiones VALUES ('DIM014', '70', '35', '22', '7');
INSERT INTO dimensiones VALUES ('DIM015', '150', '75', '45', '15');


-- ----------------------------------------------------
-- INSERT PRODUCTO
-- ----------------------------------------------------
INSERT INTO producto VALUES ('PRD001', 'Ramo de Rosas Rojas', 1, 50, 35.99, 'Ramo de rosas rojas frescas', 'DIM001');
INSERT INTO producto VALUES ('PRD002', 'Caja de Rosas Blancas', 2, 30, 25.50, 'Caja de rosas blancas para regalo', 'DIM002');
INSERT INTO producto VALUES ('PRD003', 'Bouquet de Tulipanes', 3, 40, 45.75, 'Bouquet de tulipanes variados', 'DIM003');
INSERT INTO producto VALUES ('PRD004', 'Orquídea Phalaenopsis', 4, 20, 60.00, 'Orquídea Phalaenopsis en maceta de cerámica', 'DIM004');
INSERT INTO producto VALUES ('PRD005', 'Cesta de Plantas Variadas', 5, 35, 55.25, 'Cesta de plantas variadas: suculentas, cactus, etc.', 'DIM005');
INSERT INTO producto VALUES ('PRD006', 'Ramo de Girasoles', 1, 60, 30.99, 'Ramo de girasoles frescos', 'DIM006');
INSERT INTO producto VALUES ('PRD007', 'Lavanda', 1, 25, 12.99, 'Planta de lavanda para aromaterapia', 'DIM007');
INSERT INTO producto VALUES ('PRD008', 'Helechos', 1, 40, 18.75, 'Variedad de helechos para interiores', 'DIM008');
INSERT INTO producto VALUES ('PRD009', 'Amapolas', 1, 30, 9.50, 'Flores de amapolas en varios colores', 'DIM009');
INSERT INTO producto VALUES ('PRD010', 'Pala de jardín', 2, 15, 32.50, 'Pala resistente para la jardinería', 'DIM005');
INSERT INTO producto VALUES ('PRD011', 'Tijeras de podar', 2, 50, 14.99, 'Tijeras de podar profesionales', 'DIM004');
INSERT INTO producto VALUES ('PRD012', 'Regadera', 2, 35, 20.75, 'Regadera de plástico resistente', 'DIM002');
INSERT INTO producto VALUES ('PRD013', 'Menta', 3, 20, 8.99, 'Planta de menta para infusiones', 'DIM001');
INSERT INTO producto VALUES ('PRD014', 'Romero', 3, 28, 10.50, 'Planta de romero para condimentar', 'DIM002');
INSERT INTO producto VALUES ('PRD015', 'Albahaca', 3, 22, 7.75, 'Planta de albahaca para cocina', 'DIM003');
INSERT INTO producto VALUES ('PRD016', 'Manzano', 4, 18, 45.99, 'Árbol de manzanas dulces', 'DIM004');
INSERT INTO producto VALUES ('PRD017', 'Limón', 4, 25, 30.50, 'Árbol de limones para el jardín', 'DIM005');
INSERT INTO producto VALUES ('PRD018', 'Naranjo', 4, 20, 35.75, 'Árbol de naranjas frescas', 'DIM010');
INSERT INTO producto VALUES ('PRD019', 'Dalia', 5, 32, 22.99, 'Flores de dalia en varios colores', 'DIM008');
INSERT INTO producto VALUES ('PRD020', 'Azalea', 5, 28, 28.50, 'Planta de azalea para jardines', 'DIM004');
INSERT INTO producto VALUES ('PRD021', 'Begonia', 5, 35, 15.75, 'Flores de begonia en maceta', 'DIM009');
INSERT INTO producto VALUES ('PRD023', 'Caléndula', 4, 30, 12.50, 'Flores de caléndula para jardinería', 'DIM002');
INSERT INTO producto VALUES ('PRD024', 'Petunia', 5, 35, 17.75, 'Flores de petunia para macetas', 'DIM005');
INSERT INTO producto VALUES ('PRD025', 'Semillas de Tomate', 4, 50, 5.99, 'Paquete de semillas de tomate', 'DIM013');
INSERT INTO producto VALUES ('PRD026', 'Kit de Plantación', 2, 60, 29.99, 'Kit completo para plantar en el jardín', 'DIM003');
INSERT INTO producto VALUES ('PRD027', 'Fertilizante Orgánico', 5, 45, 8.75, 'Fertilizante natural para plantas', 'DIM004');
INSERT INTO producto VALUES ('PRD028', 'Terrario de Suculentas', 5, 20, 42.50, 'Terrario con variedad de suculentas', 'DIM005');
INSERT INTO producto VALUES ('PRD029', 'Kit de Jardinería Infantil', 2, 15, 19.99, 'Kit de jardinería para niños', 'DIM002');
INSERT INTO producto VALUES ('PRD030', 'Set de Macetas de Cerámica', 5, 40, 55.99, 'Set de macetas de colores', 'DIM015');


-- ----------------------------------------------------
-- INSERT PROVEEDORES
-- ----------------------------------------------------
INSERT INTO proveedor VALUES (1, 'FloraLand', 'floraland@example.com');
INSERT INTO proveedor VALUES (2, 'PetalsRUs', 'petalsrus@example.com');
INSERT INTO proveedor VALUES (3, 'GardenDelights', 'gardendelights@example.com');
INSERT INTO proveedor VALUES (4, 'BloomMagic', 'bloommagic@example.com');
INSERT INTO proveedor VALUES (5, 'EvergreenSupplies', 'evergreensupplies@example.com');
INSERT INTO proveedor VALUES (6, 'FlowerFusion', 'flowerfusion@example.com');
INSERT INTO proveedor VALUES (7, 'GreenHarmony', 'greenharmony@example.com');
INSERT INTO proveedor VALUES (8, 'NatureWonders', 'naturewonders@example.com');
INSERT INTO proveedor VALUES (9, 'BlossomHaven', 'blossomhaven@example.com');
INSERT INTO proveedor VALUES (10, 'PetalPerfection', 'petalperfection@example.com');
INSERT INTO proveedor VALUES (11, 'GardenGlow', 'gardenglow@example.com');
INSERT INTO proveedor VALUES (12, 'FloralEssence', 'floralessence@example.com');
INSERT INTO proveedor VALUES (13, 'SunriseBlooms', 'sunriseblooms@example.com');
INSERT INTO proveedor VALUES (14, 'GoldenGardens', 'goldengardens@example.com');
INSERT INTO proveedor VALUES (15, 'PetuniaParadise', 'petuniaparadise@example.com');
INSERT INTO proveedor VALUES (16, 'MorningMeadows', 'morningmeadows@example.com');
INSERT INTO proveedor  VALUES (17, 'FlowerField', 'flowerfield@example.com');
INSERT INTO proveedor  VALUES (18, 'BlossomBazaar', 'blossombazaar@example.com');
INSERT INTO proveedor  VALUES (19, 'GardenGlory', 'gardenglory@example.com');
INSERT INTO proveedor  VALUES (20, 'PetalPleasures', 'petalpleasures@example.com');
INSERT INTO proveedor  VALUES (21, 'BotanicBeauty', 'botanicbeauty@example.com');
INSERT INTO proveedor  VALUES (22, 'SunflowerSource', 'sunflowersource@example.com');
INSERT INTO proveedor  VALUES (23, 'GreenThumb', 'greenthumb@example.com');
INSERT INTO proveedor  VALUES (24, 'FlowerFantasy', 'flowerfantasy@example.com');
INSERT INTO proveedor  VALUES (25, 'BloomBoutique', 'bloomboutique@example.com');
INSERT INTO proveedor  VALUES (26, 'RoseRidge', 'roseridge@example.com');
INSERT INTO proveedor  VALUES (27, 'TropicalTreats', 'tropicaltreats@example.com');
INSERT INTO proveedor  VALUES (28, 'GreenGroves', 'greengroves@example.com');
INSERT INTO proveedor  VALUES (29, 'FlowerFiesta', 'flowerfiesta@example.com');
INSERT INTO proveedor  VALUES (30, 'PetalPavilion', 'petalpavilion@example.com');
INSERT INTO proveedor  VALUES (31, 'LushLandscapes', 'lushlandscapes@example.com');
INSERT INTO proveedor  VALUES (32, 'BlossomBounty', 'blossombounty@example.com');


-- ----------------------------------------------------
-- INSERT PRODUCTO PROVEEDOR
-- ----------------------------------------------------

INSERT INTO producto_proveedor VALUES ('PRD001', 1, 15.99);
INSERT INTO producto_proveedor VALUES ('PRD002', 2, 20.50);
INSERT INTO producto_proveedor VALUES ('PRD003', 3, 18.75);
INSERT INTO producto_proveedor VALUES ('PRD004', 4, 12.99);
INSERT INTO producto_proveedor VALUES ('PRD005', 5, 9.99);
INSERT INTO producto_proveedor VALUES ('PRD006', 6, 14.25);
INSERT INTO producto_proveedor VALUES ('PRD007', 7, 16.50);
INSERT INTO producto_proveedor VALUES ('PRD008', 8, 22.99);
INSERT INTO producto_proveedor VALUES ('PRD009', 9, 19.99);
INSERT INTO producto_proveedor VALUES ('PRD010', 10, 17.25);
INSERT INTO producto_proveedor VALUES ('PRD011', 11, 13.99);
INSERT INTO producto_proveedor VALUES ('PRD012', 12, 24.50);
INSERT INTO producto_proveedor VALUES ('PRD013', 13, 28.75);
INSERT INTO producto_proveedor VALUES ('PRD014', 14, 32.99);
INSERT INTO producto_proveedor VALUES ('PRD015', 15, 29.99);
INSERT INTO producto_proveedor VALUES ('PRD016', 16, 26.25);
INSERT INTO producto_proveedor VALUES ('PRD017', 17, 18.99);
INSERT INTO producto_proveedor VALUES ('PRD018', 18, 21.50);
INSERT INTO producto_proveedor VALUES ('PRD019', 19, 17.75);
INSERT INTO producto_proveedor VALUES ('PRD020', 20, 14.99);
INSERT INTO producto_proveedor VALUES ('PRD021', 21, 11.99);
INSERT INTO producto_proveedor VALUES ('PRD025', 25, 22.75);
INSERT INTO producto_proveedor VALUES ('PRD026', 26, 19.99);
INSERT INTO producto_proveedor VALUES ('PRD027', 27, 15.99);
INSERT INTO producto_proveedor VALUES ('PRD028', 28, 18.50);
INSERT INTO producto_proveedor VALUES ('PRD029', 29, 23.75);
INSERT INTO producto_proveedor VALUES ('PRD030', 30, 27.99);


-- -----------------------------------------------------
-- INSERT ESTADO
-- ----------------------------------------------------

INSERT INTO estado VALUES (01,'Entregado');
INSERT INTO estado VALUES (02,'Pendiente');
INSERT INTO estado VALUES (03,'Rechazado');


-- -----------------------------------------------------
-- INSERT PAIS
-- ----------------------------------------------------
INSERT INTO pais VALUES ('ES','España');
INSERT INTO pais VALUES ('USA','EEUU');
INSERT INTO pais VALUES ('UK','Inglaterra');
INSERT INTO pais VALUES ('FR','Francia');
INSERT INTO pais VALUES ('AU','Australia');
INSERT INTO pais VALUES ('JP','Japón');


-- -----------------------------------------------------
-- INSERT REGION
-- ----------------------------------------------------
INSERT INTO region VALUES ('reg01','Barcelona','ES');
INSERT INTO region VALUES ('reg02','MA', 'USA');
INSERT INTO region VALUES ('reg03','EMEA','UK');
INSERT INTO region VALUES ('reg04','Madrid','ES');
INSERT INTO region VALUES ('reg05','EMEA','FR');
INSERT INTO region VALUES ('reg06','CA','USA');
INSERT INTO region VALUES ('reg07','APAC','AU');
INSERT INTO region VALUES ('reg08','Castilla-LaMancha','ES');
INSERT INTO region VALUES ('reg09','Chiyoda-Ku','JP');


-- -----------------------------------------------------
-- INSERT CIUDAD
-- ----------------------------------------------------
INSERT INTO ciudad VALUES (001,'Barcelona', 'reg01');
INSERT INTO ciudad VALUES (002,'Boston', 'reg02');
INSERT INTO ciudad VALUES (003,'Londres', 'reg03');
INSERT INTO ciudad VALUES (004,'Madrid', 'reg04');
INSERT INTO ciudad VALUES (005,'Paris', 'reg05');
INSERT INTO ciudad VALUES (006,'San Francisco', 'reg06');
INSERT INTO ciudad VALUES (007,'Sydney', 'reg07');
INSERT INTO ciudad VALUES (008,'Talavera de la Reina', 'reg08');
INSERT INTO ciudad VALUES (009,'Tokyo', 'reg09');


-- ----------------------------------------------------
-- INSERT OFICINA
-- ----------------------------------------------------
INSERT INTO oficina VALUES (001, 001, 000001);
INSERT INTO oficina VALUES (002, 002, 000002);
INSERT INTO oficina VALUES (003, 003, 000003);
INSERT INTO oficina VALUES (004, 004, 000004);
INSERT INTO oficina VALUES (005, 005, 000005);
INSERT INTO oficina VALUES (006, 006, 000006);
INSERT INTO oficina VALUES (007, 007, 000007);
INSERT INTO oficina VALUES (008, 008, 000008);
INSERT INTO oficina VALUES (009, 009, 000009);


-- -----------------------------------------------------
-- INSERT PUESTO EMPLEADO
-- ----------------------------------------------------

INSERT INTO puesto_empleado VALUES (1,'Director General',125);
INSERT INTO puesto_empleado VALUES (2,'Subdirector Marketing', 698);
INSERT INTO puesto_empleado VALUES (3,'Subdirector Ventas', 256);
INSERT INTO puesto_empleado VALUES (4,'Secretaria', 569);
INSERT INTO puesto_empleado VALUES (5,'Representante Ventas', 256);
INSERT INTO puesto_empleado VALUES (6,'Director Oficina', 1102);


-- ----------------------------------------------------
-- INSERT EMPLEADO 
-- ----------------------------------------------------
INSERT INTO empleado VALUES (1,'Marcos','Magaña','Perez','marcos@jardineria.es',008,NULL,1);
INSERT INTO empleado VALUES (2,'Ruben','López','Martinez','rlopez@jardineria.es',008,1,2);
INSERT INTO empleado VALUES (3,'Alberto','Soria','Carrasco','asoria@jardineria.es',008,2,3);
INSERT INTO empleado VALUES (4,'Maria','Solís','Jerez','msolis@jardineria.es',008,2,4);
INSERT INTO empleado VALUES (5,'Felipe','Rosas','Marquez','frosas@jardineria.es',008,3,5);
INSERT INTO empleado VALUES (6,'Juan Carlos','Ortiz','Serrano','cortiz@jardineria.es',008,3,5);
INSERT INTO empleado VALUES (7,'Carlos','Soria','Jimenez','csoria@jardineria.es',004,3,6);
INSERT INTO empleado VALUES (8,'Mariano','López','Murcia','mlopez@jardineria.es',004,7,5);
INSERT INTO empleado VALUES (9,'Lucio','Campoamor','Martín','lcampoamor@jardineria.es',004,7,5);
INSERT INTO empleado VALUES (10,'Hilario','Rodriguez','Huertas','hrodriguez@jardineria.es',004,7,5);
INSERT INTO empleado VALUES (11,'Emmanuel','Magaña','Perez','manu@jardineria.es',001,3,6);
INSERT INTO empleado VALUES (12,'José Manuel','Martinez','De la Osa','jmmart@hotmail.es',001,11,5);
INSERT INTO empleado VALUES (13,'David','Palma','Aceituno','dpalma@jardineria.es',001,11,5);
INSERT INTO empleado VALUES (14,'Oscar','Palma','Aceituno','opalma@jardineria.es',001,11,5);
INSERT INTO empleado VALUES (15,'Francois','Fignon','','ffignon@gardening.com',005,3,6);
INSERT INTO empleado VALUES (16,'Lionel','Narvaez','','lnarvaez@gardening.com',005,15,5);
INSERT INTO empleado VALUES (17,'Laurent','Serra','','lserra@gardening.com',005,15,5);
INSERT INTO empleado VALUES (18,'Michael','Bolton','','mbolton@gardening.com',006,3,6);
INSERT INTO empleado VALUES (19,'Walter Santiago','Sanchez','Lopez','wssanchez@gardening.com',006,18,5);
INSERT INTO empleado VALUES (20,'Hilary','Washington','','hwashington@gardening.com',002,3,6);
INSERT INTO empleado VALUES (21,'Marcus','Paxton','','mpaxton@gardening.com',002,20,5);
INSERT INTO empleado VALUES (22,'Lorena','Paxton','','lpaxton@gardening.com',002,20,5);
INSERT INTO empleado VALUES (23,'Nei','Nishikori','','nnishikori@gardening.com',009,3,6);
INSERT INTO empleado VALUES (24,'Narumi','Riko','','nriko@gardening.com',009,23,5);
INSERT INTO empleado VALUES (25,'Takuma','Nomura','','tnomura@gardening.com',009,23,5);
INSERT INTO empleado VALUES (26,'Amy','Johnson','','ajohnson@gardening.com',003,3,6);
INSERT INTO empleado VALUES (27,'Larry','Westfalls','','lwestfalls@gardening.com',003,26,5);
INSERT INTO empleado VALUES (28,'John','Walton','','jwalton@gardening.com',003,26,5);
INSERT INTO empleado VALUES (29,'Kevin','Fallmer','','kfalmer@gardening.com',007,3,6);
INSERT INTO empleado VALUES (30,'Julian','Bellinelli','','jbellinelli@gardening.com',007,29,5);
INSERT INTO empleado VALUES (31,'Mariko','Kishi','','mkishi@gardening.com',007,29,5);


-- ----------------------------------------------------
-- INSERT CLIENTE
-- ----------------------------------------------------
INSERT INTO cliente VALUES (1, 'GoldFish Garden',001, '24006', 3000,5);
INSERT INTO cliente VALUES (3, 'Gardening Associates',001, '24010', 6000,6);
INSERT INTO cliente VALUES (4, 'Gerudo Valley',001, '85495', 12000,9);
INSERT INTO cliente VALUES (5, 'Tendo Garden',002, '696969', 600000,8);
INSERT INTO cliente VALUES (6, 'Lasas S.A.',002, '28945', 154310,12);
INSERT INTO cliente VALUES (7, 'Beragua',003, '28942', 20000,13);
INSERT INTO cliente VALUES (8, 'Club Golf Puerta del hierro',003, '28930', 40000,14);
INSERT INTO cliente VALUES (9, 'Naturagua',004, '28947', 32000,16);
INSERT INTO cliente VALUES (10, 'DaraDistribuciones',009, '28946', 50000,16);
INSERT INTO cliente VALUES (11, 'Madrileña de riegos',008, '28943', 20000,17);
INSERT INTO cliente VALUES (12, 'Lasas S.A.',005, '28945', '15431',19);
INSERT INTO cliente VALUES (13, 'Camunas Jardines S.L.',009, '28145', 16481,21);
INSERT INTO cliente VALUES (14, 'Dardena S.A.',006, '28003', 321000,22);
INSERT INTO cliente VALUES (15, 'Jardin de Flores',005,'28950', 40000,24);
INSERT INTO cliente VALUES (16, 'Flores Marivi', 003,'28945', 1500,24);
INSERT INTO cliente VALUES (17, 'Flowers, S.A', 002,'24586', 3500,25);
INSERT INTO cliente VALUES (18, 'Naturajardin',001, '28011', 5050,27);
INSERT INTO cliente VALUES (19, 'Golf S.A.',009, '38297', 30000,28);
INSERT INTO cliente VALUES (20, 'Americh Golf Management SL',008, '12320', 20000,30);
INSERT INTO cliente VALUES (21, 'Aloha', 006,'35488', 50000,31);
INSERT INTO cliente VALUES (22, 'El Prat', 005,'12320', 30000,10);
INSERT INTO cliente VALUES (23, 'Sotogrande', 002,'11310', 60000,16);
INSERT INTO cliente VALUES (24, 'Vivero Humanes',001, '28970', 7430,24);
INSERT INTO cliente VALUES (25, 'Fuenla City', 007, '28574', 4500,24);
INSERT INTO cliente VALUES (26, 'Jardines y Mansiones Cactus SL',006, '29874', 76000,25);
INSERT INTO cliente VALUES (27, 'Jardinerías Matías SL', 005,'37845', 100500,24);
INSERT INTO cliente VALUES (28, 'Agrojardin',003, '28904', 8040,30);
INSERT INTO cliente VALUES (29, 'Top Campo', 002,'28574', 5500,31);
INSERT INTO cliente VALUES (30, 'Jardineria Sara',004, '27584', 7500,10);
INSERT INTO cliente VALUES (31, 'Campohermoso', 008,'28945', 3250,5);
INSERT INTO cliente VALUES (32, 'france telecom',006, '75010', 10000,6);
INSERT INTO cliente VALUES (33, 'Musée du Louvre', 002,'75058', 30000,9);
INSERT INTO cliente VALUES (35, 'Tutifruti S.A',001, '2000', 10000,24);
INSERT INTO cliente VALUES (36, 'Flores S.L.',009, '29643', 6000,12);
INSERT INTO cliente VALUES (37, 'The Magic Garden', 008,'65930', 10000,8);
INSERT INTO cliente VALUES (38, 'El Jardin Viviente S.L',005, '2003', 8000,24);


-- ----------------------------------------------------
-- INSERT PEDIDO
-- ----------------------------------------------------

-- Inserts para la tabla pedido
INSERT INTO pedido VALUES ('PED001', '2024-01-10', '2024-01-15', '2024-01-15', 'Sin comentarios', 1, 1);
INSERT INTO pedido VALUES ('PED002', '2024-01-11', '2024-01-16', '2024-01-16', 'Sin comentarios', 7, 2);
INSERT INTO pedido VALUES ('PED003', '2024-01-12', '2024-01-17', '2024-01-17', 'Sin comentarios', 3, 3);
INSERT INTO pedido VALUES ('PED004', '2024-01-13', '2024-01-18', NULL, 'Sin comentarios', 4, 1);
INSERT INTO pedido VALUES ('PED005', '2024-01-14', '2024-01-19', '2024-01-19', 'Sin comentarios', 5, 2);
INSERT INTO pedido VALUES ('PED006', '2024-01-15', '2024-01-20', '2024-01-20', 'Sin comentarios', 6, 3);
INSERT INTO pedido VALUES ('PED007', '2024-01-16', '2024-01-21', NULL, 'Sin comentarios', 7, 1);
INSERT INTO pedido VALUES ('PED008', '2024-01-17', '2024-01-22', '2024-01-22', 'Sin comentarios', 8, 2);
INSERT INTO pedido VALUES ('PED009', '2024-01-18', '2024-01-23', '2024-01-23', 'Sin comentarios', 9, 3);
INSERT INTO pedido VALUES ('PED010', '2024-01-19', '2024-01-24', NULL, 'Sin comentarios', 10, 1);
INSERT INTO pedido VALUES ('PED011', '2024-01-20', '2024-01-25', '2024-01-25', 'Sin comentarios', 11, 2);
INSERT INTO pedido VALUES ('PED012', '2024-01-21', '2024-01-26', NULL, 'Sin comentarios', 12, 3);
INSERT INTO pedido VALUES ('PED013', '2024-01-22', '2024-01-27', '2024-01-27', 'Sin comentarios', 13, 1);
INSERT INTO pedido VALUES ('PED014', '2024-01-23', '2024-01-28', '2024-01-28', 'Sin comentarios', 14, 2);
INSERT INTO pedido VALUES ('PED015', '2024-01-24', '2024-01-29', NULL, 'Sin comentarios', 15, 3);
INSERT INTO pedido VALUES ('PED016', '2024-01-25', '2024-01-30', '2024-01-30', 'Sin comentarios', 16, 1);
INSERT INTO pedido VALUES ('PED017', '2024-01-26', '2024-01-31', '2024-01-31', 'Sin comentarios', 17, 2);
INSERT INTO pedido VALUES ('PED018', '2024-01-27', '2024-02-01', NULL, 'Sin comentarios', 18, 3);
INSERT INTO pedido VALUES ('PED019', '2024-01-28', '2024-02-02', '2024-02-02', 'Sin comentarios', 19, 1);
INSERT INTO pedido VALUES ('PED020', '2024-01-29', '2024-02-03', '2024-02-03', 'Sin comentarios', 20, 2);
INSERT INTO pedido VALUES ('PED021', '2009-01-30', '2009-02-04', '2009-02-04', 'Sin comentarios', 21, 1);
INSERT INTO pedido VALUES ('PED022', '2009-01-31', '2009-02-05', '2009-02-05', 'Sin comentarios', 22, 2);
INSERT INTO pedido VALUES ('PED023', '2009-02-01', '2009-02-06', '2009-02-06', 'Sin comentarios', 23, 3);
INSERT INTO pedido VALUES ('PED024', '2009-02-02', '2009-02-07', '2009-02-07', 'Sin comentarios', 24, 1);
INSERT INTO pedido VALUES ('PED025', '2009-02-03', '2009-02-08', '2009-02-08', 'Sin comentarios', 25, 2);
INSERT INTO pedido VALUES ('PED026', '2009-02-04', '2009-02-09', '2009-02-09', 'Sin comentarios', 26, 3);
INSERT INTO pedido VALUES ('PED027', '2009-02-05', '2009-02-10', '2009-02-10', 'Sin comentarios', 27, 1);
INSERT INTO pedido VALUES ('PED028', '2009-02-06', '2009-02-11', '2009-02-11', 'Sin comentarios', 28, 2);
INSERT INTO pedido VALUES ('PED029', '2009-02-07', '2009-02-12', '2009-02-12', 'Sin comentarios', 29, 3);
INSERT INTO pedido VALUES ('PED030', '2009-02-08', '2009-02-13', '2009-02-13', 'Sin comentarios', 30, 1);
INSERT INTO pedido VALUES ('PED031', '2009-02-09', '2009-02-14', '2009-02-14', 'Sin comentarios', 31, 2);
INSERT INTO pedido VALUES ('PED032', '2009-02-10', '2009-02-15', '2009-02-15', 'Sin comentarios', 32, 3);
INSERT INTO pedido VALUES ('PED033', '2009-02-11', '2009-02-16', '2009-02-16', 'Sin comentarios', 33, 1);
INSERT INTO pedido VALUES ('PED034', '2009-02-12', '2009-02-17', '2009-02-17', 'Sin comentarios', 35, 2);
INSERT INTO pedido VALUES ('PED035', '2009-02-13', '2009-02-18', '2009-02-18', 'Sin comentarios', 35, 3);
INSERT INTO pedido VALUES ('PED036', '2009-02-14', '2009-02-19', '2009-02-19', 'Sin comentarios', 36, 1);
INSERT INTO pedido VALUES ('PED037', '2009-02-15', '2009-02-20', '2009-02-20', 'Sin comentarios', 37, 2);
INSERT INTO pedido VALUES ('PED038', '2009-02-16', '2009-02-21', '2009-02-21', 'Sin comentarios', 38, 3);
INSERT INTO pedido VALUES ('PED039', '2009-02-17', '2009-02-22', '2009-02-22', 'Sin comentarios', 1, 1);
INSERT INTO pedido VALUES ('PED040', '2009-02-18', '2009-02-23', '2009-02-23', 'Sin comentarios', 4, 2);


-- ----------------------------------------------------
-- INSERT DETALLE DE PEDIDO
-- ----------------------------------------------------
INSERT INTO detalle_pedido VALUES ('PRD001', 'PED001', 5, 12.50, 1);
INSERT INTO detalle_pedido VALUES ('PRD002', 'PED002', 3, 15.99, 2);
INSERT INTO detalle_pedido VALUES ('PRD003', 'PED003', 2, 8.75, 3);
INSERT INTO detalle_pedido VALUES ('PRD004', 'PED004', 4, 10.99, 4);
INSERT INTO detalle_pedido VALUES ('PRD005', 'PED005', 6, 7.99, 5);
INSERT INTO detalle_pedido VALUES ('PRD006', 'PED006', 1, 18.50, 6);
INSERT INTO detalle_pedido VALUES ('PRD007', 'PED007', 2, 14.25, 7);
INSERT INTO detalle_pedido VALUES ('PRD008', 'PED008', 3, 20.99, 8);
INSERT INTO detalle_pedido VALUES ('PRD009', 'PED009', 5, 16.75, 9);
INSERT INTO detalle_pedido VALUES ('PRD010', 'PED010', 2, 11.99, 10);
INSERT INTO detalle_pedido VALUES ('PRD011', 'PED011', 3, 9.50, 11);
INSERT INTO detalle_pedido VALUES ('PRD012', 'PED012', 4, 22.75, 12);
INSERT INTO detalle_pedido VALUES ('PRD013', 'PED013', 2, 28.99, 13);
INSERT INTO detalle_pedido VALUES ('PRD014', 'PED014', 1, 32.50, 14);
INSERT INTO detalle_pedido VALUES ('PRD015', 'PED015', 3, 29.99, 15);
INSERT INTO detalle_pedido VALUES ('PRD016', 'PED016', 4, 17.99, 16);
INSERT INTO detalle_pedido VALUES ('PRD017', 'PED017', 5, 12.75, 17);
INSERT INTO detalle_pedido VALUES ('PRD018', 'PED018', 3, 21.50, 18);
INSERT INTO detalle_pedido VALUES ('PRD019', 'PED019', 2, 19.99, 19);
INSERT INTO detalle_pedido VALUES ('PRD020', 'PED020', 6, 14.50, 20);
INSERT INTO detalle_pedido VALUES ('PRD021', 'PED021', 1, 27.75, 21);
INSERT INTO detalle_pedido VALUES ('PRD023', 'PED022', 3, 23.99, 22);
INSERT INTO detalle_pedido VALUES ('PRD023', 'PED023', 4, 30.50, 23);
INSERT INTO detalle_pedido VALUES ('PRD024', 'PED024', 2, 25.99, 24);
INSERT INTO detalle_pedido VALUES ('PRD025', 'PED025', 3, 18.75, 25);
INSERT INTO detalle_pedido VALUES ('PRD026', 'PED026', 5, 22.99, 26);
INSERT INTO detalle_pedido VALUES ('PRD027', 'PED027', 2, 26.50, 27);
INSERT INTO detalle_pedido VALUES ('PRD028', 'PED028', 3, 14.99, 28);
INSERT INTO detalle_pedido VALUES ('PRD029', 'PED029', 4, 31.75, 29);
INSERT INTO detalle_pedido VALUES ('PRD030', 'PED030', 3, 24.50, 30);
INSERT INTO detalle_pedido VALUES ('PRD025', 'PED031', 4, 30.99, 31);
INSERT INTO detalle_pedido VALUES ('PRD026', 'PED032', 2, 16.75, 32);
INSERT INTO detalle_pedido VALUES ('PRD027', 'PED033', 3, 21.99, 33);


-- -----------------------------------------------------
-- INSERT TIPO TELEFONO
-- ----------------------------------------------------
INSERT INTO tipo_telefono VALUES (1,'Celular');
INSERT INTO tipo_telefono VALUES (2,'Fax');
INSERT INTO tipo_telefono VALUES (3,'Fijo o Casa');


-- ----------------------------------------------------
-- INSERT TELEFONO CLIENTE
-- ----------------------------------------------------
INSERT INTO telefono_cliente VALUES (1, '5556901745', 1, 1);
INSERT INTO telefono_cliente VALUES (3, '5552323129', 3, 3);
INSERT INTO telefono_cliente VALUES (4, '55591233210', 4, 3);
INSERT INTO telefono_cliente VALUES (5, '34916540145', 5, 2);
INSERT INTO telefono_cliente VALUES (6, '654987321', 6, 1);
INSERT INTO telefono_cliente VALUES (7, '62456810', 7, 1);
INSERT INTO telefono_cliente VALUES (8, '689234750', 8, 3);
INSERT INTO telefono_cliente VALUES (9, '675598001', 9, 3);
INSERT INTO telefono_cliente VALUES (10, '655983045', 10, 2);
INSERT INTO telefono_cliente VALUES (11, '34914873241', 11, 1);
INSERT INTO telefono_cliente VALUES (12, '34912453217', 12, 1);
INSERT INTO telefono_cliente VALUES (14, '666555444', 14, 3);
INSERT INTO telefono_cliente VALUES (15, '698754159', 15, 2);
INSERT INTO telefono_cliente VALUES (16, '612343529', 16, 1);
INSERT INTO telefono_cliente VALUES (17, '916458762', 17, 3);
INSERT INTO telefono_cliente VALUES (18, '964493072', 18, 2);
INSERT INTO telefono_cliente VALUES (19, '916485852', 19, 1);
INSERT INTO telefono_cliente VALUES (20, '916882323', 20, 1);
INSERT INTO telefono_cliente VALUES (21, '915576622', 21, 2);
INSERT INTO telefono_cliente VALUES (22, '654987690', 22, 3);
INSERT INTO telefono_cliente VALUES (23, '675842139', 23, 2);
INSERT INTO telefono_cliente VALUES (24, '916877445', 24, 2);
INSERT INTO telefono_cliente VALUES (25, '916544147', 25, 2);
INSERT INTO telefono_cliente VALUES (26, '675432926', 26, 3);
INSERT INTO telefono_cliente VALUES (27, '685746512', 27, 1);
INSERT INTO telefono_cliente VALUES (28, '675124537', 28, 3);
INSERT INTO telefono_cliente VALUES (29, '645925376', 29, 3);
INSERT INTO telefono_cliente VALUES (30, '5120578961', 30,1);
INSERT INTO telefono_cliente VALUES (31, '0140205050', 31,2);
INSERT INTO telefono_cliente VALUES (32, '9261-2433', 32, 1);
INSERT INTO telefono_cliente VALUES (33, '654352981', 33, 1);
INSERT INTO telefono_cliente VALUES (35, '8005-7161', 35, 3);
INSERT INTO telefono_cliente VALUES (36, '8005-7162', 36, 2);


-- -----------------------------------------------------
-- INSERT TIPO DIRECCION
-- ----------------------------------------------------
INSERT INTO tipo_direccion VALUES (1,'Avenida');
INSERT INTO tipo_direccion VALUES (111,'Calle');
INSERT INTO tipo_direccion VALUES (222,'Bulevar');
INSERT INTO tipo_direccion VALUES (333,'Carrera');
INSERT INTO tipo_direccion VALUES (444,'Pasaje');


-- ----------------------------------------------------
-- INSERT DIRECCCION CLIENTE
-- ----------------------------------------------------
INSERT INTO direccion VALUES (NULL, 'False Street', '52 2 A', NULL, 1, 1);
INSERT INTO direccion VALUES (NULL, 'Wall-e Avenue', NULL, NULL, 3, 111);
INSERT INTO direccion VALUES (NULL, 'Oaks Avenue nº22', NULL, NULL, 4, 222);
INSERT INTO direccion VALUES (NULL, 'Null Street nº69', NULL, NULL, 5, 333);
INSERT INTO direccion VALUES (NULL, 'C/Leganes', '15', NULL, 6, 444);
INSERT INTO direccion VALUES (NULL, 'C/pintor segundo', NULL, NULL, 7, 1);
INSERT INTO direccion VALUES (NULL, 'C/sinesio delgado', NULL, NULL, 8, 111);
INSERT INTO direccion VALUES (NULL, 'C/majadahonda', NULL, NULL, 9, 222);
INSERT INTO direccion VALUES (NULL, 'C/azores', NULL, NULL, 10, 333);
INSERT INTO direccion VALUES (NULL, 'C/Lagañas', NULL, NULL, 11, 444);
INSERT INTO direccion VALUES (NULL, 'C/Virgenes', '45', NULL, 13, 1);
INSERT INTO direccion VALUES (NULL, 'C/Nueva York', '74', NULL, 14, 111);
INSERT INTO direccion VALUES (NULL, 'C/ Oña', '34', NULL, 15, 222);
INSERT INTO direccion VALUES (NULL, 'C/Leganes24', NULL, NULL, 16, 333);
INSERT INTO direccion VALUES (NULL, 'C/Luis Salquillo4', NULL, NULL, 17, 444);
INSERT INTO direccion VALUES (NULL, 'Plaza Magallón', '15', NULL, 18, 1);
INSERT INTO direccion VALUES (NULL, 'C/Estancado', NULL, NULL, 19, 111);
INSERT INTO direccion VALUES (NULL, 'C/Roman', '3', NULL, 20, 222);
INSERT INTO direccion VALUES (NULL, 'Avenida Tibidabo', NULL, NULL, 21, 333);
INSERT INTO direccion VALUES (NULL, 'C/Paseo del Parque', NULL, NULL, 22, 444);
INSERT INTO direccion VALUES (NULL, 'C/Miguel Echegaray', '54', NULL, 23, 1);
INSERT INTO direccion VALUES (NULL, 'C/Callo', '52', NULL, 24, 111);
INSERT INTO direccion VALUES (NULL, 'Polígono Industrial Maspalomas, Nº52', NULL, NULL, 25, 222);
INSERT INTO direccion VALUES (NULL, 'C/Francisco Arce, Nº44', NULL, NULL, 26, 333);
INSERT INTO direccion VALUES (NULL, 'C/Mar Caspio', '43', NULL, 27, 444);
INSERT INTO direccion VALUES (NULL, 'C/Ibiza', '32', NULL, 28, 1);
INSERT INTO direccion VALUES (NULL, 'C/Lima', '1', NULL, 29, 111);
INSERT INTO direccion VALUES (NULL, 'C/Peru', '78', NULL, 30, 222);
INSERT INTO direccion VALUES (NULL, '6 place d Alleray', '15ème', NULL, 31, 333);
INSERT INTO direccion VALUES (NULL, 'Quai du Louvre', NULL, NULL, 32, 444);
INSERT INTO direccion VALUES (NULL, 'level 24, St. Martins Tower.-31 Market St.', NULL, NULL, 33, 1);


-- ----------------------------------------------------
-- INSERT CONTACTO
-- ----------------------------------------------------

INSERT INTO contacto VALUES ('GC-001', 'Daniel', 'G', 'daniel@ejemplo.com', 'García', NULL,1);
INSERT INTO contacto VALUES ('GG-003', 'Link', 'F', 'link@ejemplo.com', NULL, NULL,3);
INSERT INTO contacto VALUES ('TG-004', 'Akane', 'T', 'akane@ejemplo.com', NULL, NULL,4);
INSERT INTO contacto VALUES ('LAS-005', 'Antonio', 'L', 'antonio@ejemplo.com', NULL, NULL,5);
INSERT INTO contacto VALUES ('BJ-006', 'Jose', 'B', 'jose@ejemplo.com', NULL, NULL,6);
INSERT INTO contacto VALUES ('PL-007', 'Paco', 'L', 'paco@ejemplo.com', NULL, NULL,7);
INSERT INTO contacto VALUES ('GR-008', 'Guillermo', 'R', 'guillermo@ejemplo.com', NULL, NULL,8);
INSERT INTO contacto VALUES ('DS-009', 'David', 'S', 'david@ejemplo.com', NULL, NULL,9);
INSERT INTO contacto VALUES ('JT-010', 'Jose', 'T', 'jose@ejemplo.com', NULL, NULL,10);
INSERT INTO contacto VALUES ('LAS-011', 'Antonio', 'L', 'antonio@ejemplo.com', NULL, NULL,11);
INSERT INTO contacto VALUES ('PC-012', 'Pedro', 'C', 'pedro@ejemplo.com', NULL, NULL,12);
INSERT INTO contacto VALUES ('JR-013', 'Juan', 'R', 'juan@ejemplo.com', NULL, NULL,13);
INSERT INTO contacto VALUES ('JV-014', 'Javier', 'V', 'javier@ejemplo.com', NULL, NULL,14);
INSERT INTO contacto VALUES ('MR-015', 'Maria', 'R', 'maria@ejemplo.com', NULL, NULL,15);
INSERT INTO contacto VALUES ('BF-016', 'Beatriz', 'F', 'beatriz@ejemplo.com', NULL, NULL,16);
INSERT INTO contacto VALUES ('VC-017', 'Victoria', 'C', 'victoria@ejemplo.com', NULL, NULL,17);
INSERT INTO contacto VALUES ('LM-018', 'Luis', 'M', 'luis@ejemplo.com', NULL, NULL,18);
INSERT INTO contacto VALUES ('MS-019', 'Mario', 'S', 'mario@ejemplo.com', NULL, NULL,19);
INSERT INTO contacto VALUES ('CR-020', 'Cristian', 'R', 'cristian@ejemplo.com', NULL, NULL, 20);
INSERT INTO contacto VALUES ('FC-021', 'Francisco', 'C', 'francisco@ejemplo.com', NULL, NULL,21);
INSERT INTO contacto VALUES ('MS-022', 'Maria', 'S', 'maria@ejemplo.com', NULL, NULL,22);
INSERT INTO contacto VALUES ('FG-023', 'Federico', 'G', 'federico@ejemplo.com', NULL, NULL,23);
INSERT INTO contacto VALUES ('TM-024', 'Tony', 'M', 'tony@ejemplo.com', NULL, NULL, 24);
INSERT INTO contacto VALUES ('EMS-025', 'Eva María', 'S', 'eva@ejemplo.com', NULL, NULL, 25);
INSERT INTO contacto VALUES ('MSM-026', 'Matías', 'S', 'matias@ejemplo.com', NULL, NULL, 26);
INSERT INTO contacto VALUES ('BL-027', 'Benito', 'L', 'benito@ejemplo.com', NULL, NULL, 27);
INSERT INTO contacto VALUES ('JS-028', 'Joseluis', 'S', 'joseluis@ejemplo.com', NULL, NULL, 28);
INSERT INTO contacto VALUES ('SM-029', 'Sara', 'M', 'sara@ejemplo.com', NULL, NULL, 29);
INSERT INTO contacto VALUES ('LJ-030', 'Luis', 'J', 'luis@ejemplo.com', NULL, NULL, 30);
INSERT INTO contacto VALUES ('FT-031', 'Fraçois', 'T', 'francois@ejemplo.com', NULL, NULL, 31);
INSERT INTO contacto VALUES ('PD-032', 'Pierre', 'D', 'pierre@ejemplo.com', NULL, NULL, 32);
INSERT INTO contacto VALUES ('JJ-033', 'Jacob', 'J', 'jacob@ejemplo.com', NULL, NULL, 33);
INSERT INTO contacto VALUES ('RM-035', 'Richard', 'M', 'richard@ejemplo.com', NULL, NULL, 35);
INSERT INTO contacto VALUES ('JS-036', 'Justin', 'S', 'justin@ejemplo.com', NULL, NULL, 36);
INSERT INTO contacto VALUES ('GA-002', 'Anne', 'W', 'anne@ejemplo.com', NULL, NULL,37);
INSERT INTO contacto VALUES ('AR-034', 'Antonio', 'R', 'antonio@ejemplo.com', NULL, NULL, 38);


-- -----------------------------------------------------
-- INSERT METODO PAGO
-- ----------------------------------------------------
INSERT INTO metodo_pago VALUES (1,'Transfrerencia');
INSERT INTO metodo_pago VALUES (2,'Cheque');
INSERT INTO metodo_pago VALUES (3,'efectivo');
INSERT INTO metodo_pago VALUES (4,'Paypal');


-- ----------------------------------------------------
-- INSERT PAGO
-- ----------------------------------------------------

INSERT INTO pago  VALUES (1, '2008-11-10', 2000, 1, 1);
INSERT INTO pago  VALUES (2, '2008-12-10', 2000, 1, 2);
INSERT INTO pago  VALUES (3, '2009-01-16', 5000, 3, 3);
INSERT INTO pago  VALUES (4, '2009-02-16', 5000, 3, 1);
INSERT INTO pago  VALUES (5, '2009-02-19', 926, 3, 3);
INSERT INTO pago  VALUES (6, '2007-01-08', 20000, 4, 3);
INSERT INTO pago  VALUES (7, '2007-01-08', 20000, 4, 2);
INSERT INTO pago  VALUES (8, '2007-01-08', 20000, 4, 3);
INSERT INTO pago  VALUES (9, '2007-01-08', 20000, 4, 2);
INSERT INTO pago  VALUES (10, '2007-01-08', 1849, 4, 3);
INSERT INTO pago  VALUES (11, '2006-01-18', 23794, 5, 2);
INSERT INTO pago  VALUES (12, '2009-01-13', 2390, 7, 1);
INSERT INTO pago  VALUES (13, '2009-01-06', 929, 9, 2);
INSERT INTO pago  VALUES (14, '2008-08-04', 2246, 13, 2);
INSERT INTO pago  VALUES (15, '2008-07-15', 4160, 14, 3);
INSERT INTO pago  VALUES (16, '2009-01-15', 2081, 15, 1);
INSERT INTO pago  VALUES (17, '2009-02-15', 10000, 15, 1);
INSERT INTO pago  VALUES (18, '2009-02-16', 4399, 16, 1);
INSERT INTO pago  VALUES (19, '2009-03-06', 232, 19, 3);
INSERT INTO pago  VALUES (20, '2009-03-26', 272, 23, 2);
INSERT INTO pago  VALUES (21, '2008-03-18', 18846, 26, 2);
INSERT INTO pago  VALUES (22, '2009-02-08', 10972, 27, 2);
INSERT INTO pago  VALUES (23, '2009-01-13', 8489, 28, 3);
INSERT INTO pago  VALUES (24, '2009-01-16', 7863, 30, 1);
INSERT INTO pago  VALUES (25, '2007-10-06', 3321, 35, 2);
INSERT INTO pago  VALUES (26, '2006-05-26', 1171, 38, 3);


-- ----------------------------------------------------
-- INSERT DIRECCION OFICINA
-- ----------------------------------------------------

INSERT INTO direccion_oficina VALUES (1,'Avenida Diagonal', '38', NULL,001, 1);
INSERT INTO direccion_oficina VALUES (2,'Court Place', '1550', NULL, 002, 111);
INSERT INTO direccion_oficina VALUES (3,'Old Broad Street', '52', NULL, 003, 222);
INSERT INTO direccion_oficina VALUES (4,'Bulevar Indalecio Prieto', '32', NULL, 004, 222);
INSERT INTO direccion_oficina VALUES (5,'Rue Jouffroy d''abbans', '29', NULL, 005, 333);
INSERT INTO direccion_oficina VALUES (6,'Market Street', '100', NULL, 006, 111);
INSERT INTO direccion_oficina VALUES (7,'Wentworth Avenue', '5-11', NULL, 007, 444);
INSERT INTO direccion_oficina VALUES (8,'Francisco Aguirre', '32', NULL, 008, 1);
INSERT INTO direccion_oficina VALUES (9,'Kioicho', '4-1', NULL, 009, 111);

-- ----------------------------------------------------
-- INSERT TELEFONO OFICINA
-- ----------------------------------------------------

INSERT INTO telefono_oficina VALUES (NULL, '+34 93 3561182', 1, 001);
INSERT INTO telefono_oficina VALUES (NULL, '+1 215 837 0825', 2, 002);
INSERT INTO telefono_oficina VALUES (NULL, '+44 20 78772041', 3, 003);
INSERT INTO telefono_oficina VALUES (NULL,'+34 91 7514487', 2, 004);
INSERT INTO telefono_oficina VALUES (NULL, '+33 14 723 4404', 2, 005);
INSERT INTO telefono_oficina VALUES (NULL, '+1 650 219 4782', 1, 006);
INSERT INTO telefono_oficina VALUES (NULL, '+61 2 9264 2451', 3, 007);
INSERT INTO telefono_oficina VALUES (NULL, '+34 925 867231', 1, 008);
INSERT INTO telefono_oficina VALUES (NULL, '+81 33 224 5000', 3, 009);

```



### Consultas sobre una tabla

1. Devuelve un listado con el código de oficina y la ciudad donde hay oficinas.

   ```mysql
   SELECT o.codigo_oficina, c.nombre_ciudad
   FROM oficina o
   JOIN ciudad c ON o.codigo_ciudad = c.codigo_ciudad;
   +----------------+----------------------+
   | codigo_oficina | nombre_ciudad        |
   +----------------+----------------------+
   |              1 | Barcelona            |
   |              2 | Boston               |
   |              3 | Londres              |
   |              4 | Madrid               |
   |              5 | Paris                |
   |              6 | San Francisco        |
   |              7 | Sydney               |
   |              8 | Talavera de la Reina |
   |              9 | Tokyo                |
   +----------------+----------------------+
   9 rows in set (0.00 sec)
   ```

   

2. Devuelve un listado con la ciudad y el teléfono de las oficinas de España.

   ```mysql
   SELECT c.nombre_ciudad, tof.telefono_oficina
   FROM ciudad c
   JOIN region r ON c.codigo_region = r.codigo_region
   JOIN pais p ON r.codigo_pais = p.codigo_pais
   JOIN oficina o ON c.codigo_ciudad = o.codigo_ciudad
   JOIN telefono_oficina tof ON o.codigo_oficina = tof.codigo_oficina
   WHERE p.nombre_pais = 'España';
   +----------------------+------------------+
   | nombre_ciudad        | telefono_oficina |
   +----------------------+------------------+
   | Barcelona            | +34 93 3561182   |
   | Madrid               | +34 91 7514487   |
   | Talavera de la Reina | +34 925 867231   |
   +----------------------+------------------+
   3 rows in set (0.00 sec)
   ```

   

3. Devuelve un listado con el nombre, apellidos y email de los empleados cuyo jefe tiene un código de jefe igual a 7.

   ```mysql
   SELECT e.nombre_empleado, e.apellido1_empleado, e.apellido2_empleado, e.email_empleado
   FROM empleado e
   WHERE e.codigo_jefe = 7;
   +-----------------+--------------------+--------------------+--------------------------+
   | nombre_empleado | apellido1_empleado | apellido2_empleado | email_empleado           |
   +-----------------+--------------------+--------------------+--------------------------+
   | Mariano         | López              | Murcia             | mlopez@jardineria.es     |
   | Lucio           | Campoamor          | Martín             | lcampoamor@jardineria.es |
   | Hilario         | Rodriguez          | Huertas            | hrodriguez@jardineria.es |
   +-----------------+--------------------+--------------------+--------------------------+
   3 rows in set (0.00 sec)
   ```

   

4. Devuelve el nombre del puesto, nombre, apellidos y email del jefe de la empresa.

   ```mysql
   SELECT pe.nombre_puesto, CONCAT(e.nombre_empleado,' ', e.apellido1_empleado,' ', ifnull(e.apellido2_empleado,'')) AS nombre_empleado, e.email_empleado
   FROM empleado e
   JOIN puesto_empleado pe ON e.codigo_puesto_empleado = pe.codigo_puesto_empleado
   WHERE e.codigo_jefe IS NULL;
   +------------------+---------------------+----------------------+
   | nombre_puesto    | nombre_empleado     | email_empleado       |
   +------------------+---------------------+----------------------+
   | Director General | Marcos Magaña Perez | marcos@jardineria.es |
   +------------------+---------------------+----------------------+
   1 row in set (0.00 sec)
   ```

   

5. Devuelve un listado con el nombre, apellidos y puesto de aquellos empleados que no sean representantes de ventas.

   ```mysql
   SELECT e.nombre_empleado, e.apellido1_empleado, e.apellido2_empleado, pe.nombre_puesto
   FROM empleado e
   JOIN puesto_empleado pe ON e.codigo_puesto_empleado = pe.codigo_puesto_empleado
   WHERE pe.nombre_puesto != 'Representante de ventas';
   +-----------------+--------------------+--------------------+-----------------------+
   | nombre_empleado | apellido1_empleado | apellido2_empleado | nombre_puesto         |
   +-----------------+--------------------+--------------------+-----------------------+
   | Marcos          | Magaña             | Perez              | Director General      |
   | Ruben           | López              | Martinez           | Subdirector Marketing |
   | Alberto         | Soria              | Carrasco           | Subdirector Ventas    |
   | Maria           | Solís              | Jerez              | Secretaria            |
   | Felipe          | Rosas              | Marquez            | Representante Ventas  |
   | Juan Carlos     | Ortiz              | Serrano            | Representante Ventas  |
   | Mariano         | López              | Murcia             | Representante Ventas  |
   | Lucio           | Campoamor          | Martín             | Representante Ventas  |
   | Hilario         | Rodriguez          | Huertas            | Representante Ventas  |
   | José Manuel     | Martinez           | De la Osa          | Representante Ventas  |
   | David           | Palma              | Aceituno           | Representante Ventas  |
   | Oscar           | Palma              | Aceituno           | Representante Ventas  |
   | Lionel          | Narvaez            |                    | Representante Ventas  |
   | Laurent         | Serra              |                    | Representante Ventas  |
   | Walter Santiago | Sanchez            | Lopez              | Representante Ventas  |
   | Marcus          | Paxton             |                    | Representante Ventas  |
   | Lorena          | Paxton             |                    | Representante Ventas  |
   | Narumi          | Riko               |                    | Representante Ventas  |
   | Takuma          | Nomura             |                    | Representante Ventas  |
   | Larry           | Westfalls          |                    | Representante Ventas  |
   | John            | Walton             |                    | Representante Ventas  |
   | Julian          | Bellinelli         |                    | Representante Ventas  |
   | Mariko          | Kishi              |                    | Representante Ventas  |
   | Carlos          | Soria              | Jimenez            | Director Oficina      |
   | Emmanuel        | Magaña             | Perez              | Director Oficina      |
   | Francois        | Fignon             |                    | Director Oficina      |
   | Michael         | Bolton             |                    | Director Oficina      |
   | Hilary          | Washington         |                    | Director Oficina      |
   | Nei             | Nishikori          |                    | Director Oficina      |
   | Amy             | Johnson            |                    | Director Oficina      |
   | Kevin           | Fallmer            |                    | Director Oficina      |
   +-----------------+--------------------+--------------------+-----------------------+
   31 rows in set (0.00 sec)
   ```

   

6. Devuelve un listado con el nombre de los todos los clientes españoles.

   ```mysql
   SELECT cli.nombre_cliente
   FROM cliente cli
   JOIN ciudad c ON cli.codigo_ciudad = c.codigo_ciudad
   JOIN region r ON c.codigo_region = r.codigo_region
   JOIN pais p ON r.codigo_pais = p.codigo_pais
   WHERE p.nombre_pais = 'España';
   +----------------------------+
   | nombre_cliente             |
   +----------------------------+
   | GoldFish Garden            |
   | Gardening Associates       |
   | Gerudo Valley              |
   | Naturajardin               |
   | Vivero Humanes             |
   | Tutifruti S.A              |
   | Naturagua                  |
   | Jardineria Sara            |
   | Madrileña de riegos        |
   | Americh Golf Management SL |
   | Campohermoso               |
   | The Magic Garden           |
   +----------------------------+
   12 rows in set (0.00 sec)
   ```

   

7. Devuelve un listado con los distintos estados por los que puede pasar un pedido.

   ```mysql
   SELECT nombre
   FROM estado;
   +-----------+
   | nombre    |
   +-----------+
   | Entregado |
   | Pendiente |
   | Rechazado |
   +-----------+
   3 rows in set (0.00 sec)
   ```

   

8. Devuelve un listado con el código de cliente de aquellos clientes que realizaron algún pago en 2008. Tenga en cuenta que deberá eliminar aquellos códigos de cliente que aparezcan repetidos. Resuelva la consulta:

   * Utilizando la función YEAR de MySQL.

     ```mysql
     SELECT DISTINCT codigo_cliente
     FROM pago
     WHERE YEAR(fecha_pago) = 2008;
     +----------------+
     | codigo_cliente |
     +----------------+
     |              1 |
     |             13 |
     |             14 |
     |             26 |
     +----------------+
     4 rows in set (0.00 sec)
     ```

     

   * Utilizando la función DATE_FORMAT de MySQL.

     ```mysql
     SELECT DISTINCT codigo_cliente
     FROM pago
     WHERE DATE_FORMAT(fecha_pago, '%Y') = '2008';
     Empty set (0,00 sec)
     ```

     

   * Sin utilizar ninguna de las funciones anteriores.

     ```mysql
     SELECT DISTINCT codigo_cliente
     FROM pago
     WHERE fecha_pago >= '2008-01-01' AND fecha_pago < '2009-01-01';
     +----------------+
     | codigo_cliente |
     +----------------+
     |              1 |
     |             13 |
     |             14 |
     |             26 |
     +----------------+
     4 rows in set (0.00 sec)
     ```

     

9. Devuelve un listado con el código de pedido, código de cliente, fecha esperada y fecha de entrega de los pedidos que no han sido entregados a tiempo.

   ```mysql
   SELECT codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega
   FROM pedido
   WHERE fecha_entrega > fecha_esperada;
   Empty set (0,00 sec)
   ```

   

10. Devuelve un listado con el código de pedido, código de cliente, fecha esperada y fecha de entrega de los pedidos cuya fecha de entrega ha sido al menos dos días antes de la fecha esperada.

    * Utilizando la función ADDDATE de MySQL.

      ```mysql
      SELECT codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega
      FROM pedido
      WHERE fecha_entrega < ADDDATE(fecha_esperada, -2);
      Empty set (0,00 sec)
      ```

      

    * Utilizando la función DATEDIFF de MySQL.

      ```mysql
      SELECT codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega
      FROM pedido
      WHERE DATEDIFF(fecha_esperada, fecha_entrega) >= 2;
      Empty set (0,00 sec)
      ```

      

    * ¿Sería posible resolver esta consulta utilizando el operador de suma + o resta -?

      ```mysql
      SELECT codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega
      FROM pedido
      WHERE fecha_entrega < fecha_esperada - INTERVAL 2 DAY;
      Empty set (0,00 sec)
      ```

      

11. Devuelve un listado de todos los pedidos que fueron rechazados en 2009.

    ```mysql
    SELECT codigo_pedido, fecha_pedido, fecha_entrega, comentarios
    FROM pedido
    WHERE codigo_estado = (
        SELECT codigo_estado
        FROM estado
        WHERE nombre = 'Rechazado'
    ) AND YEAR(fecha_pedido) = 2009;
    +---------------+--------------+---------------+-----------------+
    | codigo_pedido | fecha_pedido | fecha_entrega | comentarios     |
    +---------------+--------------+---------------+-----------------+
    | PED023        | 2009-02-01   | 2009-02-06    | Sin comentarios |
    | PED026        | 2009-02-04   | 2009-02-09    | Sin comentarios |
    | PED029        | 2009-02-07   | 2009-02-12    | Sin comentarios |
    | PED032        | 2009-02-10   | 2009-02-15    | Sin comentarios |
    | PED035        | 2009-02-13   | 2009-02-18    | Sin comentarios |
    | PED038        | 2009-02-16   | 2009-02-21    | Sin comentarios |
    +---------------+--------------+---------------+-----------------+
    6 rows in set (0.00 sec)
    ```

    

12. Devuelve un listado de todos los pedidos que han sido entregados en el mes de enero de cualquier año.

    ```mysql
    SELECT codigo_pedido, fecha_pedido, fecha_entrega, comentarios
    FROM pedido
    WHERE MONTH(fecha_entrega) = 1;
    +---------------+--------------+---------------+-----------------+
    | codigo_pedido | fecha_pedido | fecha_entrega | comentarios     |
    +---------------+--------------+---------------+-----------------+
    | PED001        | 2024-01-10   | 2024-01-15    | Sin comentarios |
    | PED002        | 2024-01-11   | 2024-01-16    | Sin comentarios |
    | PED003        | 2024-01-12   | 2024-01-17    | Sin comentarios |
    | PED005        | 2024-01-14   | 2024-01-19    | Sin comentarios |
    | PED006        | 2024-01-15   | 2024-01-20    | Sin comentarios |
    | PED008        | 2024-01-17   | 2024-01-22    | Sin comentarios |
    | PED009        | 2024-01-18   | 2024-01-23    | Sin comentarios |
    | PED011        | 2024-01-20   | 2024-01-25    | Sin comentarios |
    | PED013        | 2024-01-22   | 2024-01-27    | Sin comentarios |
    | PED014        | 2024-01-23   | 2024-01-28    | Sin comentarios |
    | PED016        | 2024-01-25   | 2024-01-30    | Sin comentarios |
    | PED017        | 2024-01-26   | 2024-01-31    | Sin comentarios |
    +---------------+--------------+---------------+-----------------+
    12 rows in set (0.00 sec)
    ```

    

13. Devuelve un listado con todos los pagos que se realizaron en el año 2008 mediante Paypal. Ordene el resultado de mayor a menor.

    ```mysql
    SELECT codigo_pago, fecha_pago, total_pago, codigo_cliente, codigo_met_pago
    FROM pago
    WHERE YEAR(fecha_pago) = 2008 AND codigo_met_pago = (
        SELECT codigo_metodo_pago
        FROM metodo_pago
        WHERE nombre_met_pago = 'Paypal'
    )
    ORDER BY total_pago DESC;
    Empty set (0,00 sec)
    ```

    

14. Devuelve un listado con todas las formas de pago que aparecen en la tabla pago. Tenga en cuenta que no deben aparecer formas de pago repetidas.

    ```mysql
    SELECT DISTINCT mp.codigo_metodo_pago, mp.nombre_met_pago
    FROM pago p
    JOIN metodo_pago mp ON p.codigo_met_pago = mp.codigo_metodo_pago;
    +--------------------+-----------------+
    | codigo_metodo_pago | nombre_met_pago |
    +--------------------+-----------------+
    |                  1 | Transfrerencia  |
    |                  2 | Cheque          |
    |                  3 | efectivo        |
    +--------------------+-----------------+
    3 rows in set (0.00 sec)
    ```

    

15. Devuelve un listado con todos los productos que pertenecen a la gama Ornamentales y que tienen más de 100 unidades en stock. El listado deberá estar ordenado por su precio de venta, mostrando en primer lugar los de mayor precio.

    ```mysql
    SELECT p.codigo_producto, p.nombre, p.cantidad_stock, p.precio_venta, p.descripcion, p.codigo_dimensiones
    FROM producto p
    JOIN gama_producto gp ON p.codigo_gama = gp.codigo_gama
    WHERE gp.gama = 'Ornamentales' AND p.cantidad_stock > 100
    ORDER BY p.precio_venta DESC;
    Empty set (0,00 sec)
    ```

    

16. Devuelve un listado con todos los clientes que sean de la ciudad de Madrid y cuyo representante de ventas tenga el código de empleado 11 o 30.

    ```mysql
    SELECT c.codigo_cliente, c.nombre_cliente, c.codigo_ciudad, c.codigo_postal, c.limite_credito, c.codigo_rep_ventas
    FROM cliente c
    JOIN ciudad ci ON c.codigo_ciudad = ci.codigo_ciudad
    JOIN empleado e ON c.codigo_rep_ventas = e.codigo_empleado
    WHERE ci.nombre_ciudad = 'Madrid' AND e.codigo_empleado IN (11, 30);
    Empty set (0,00 sec)
    ```

    

### Consultas multitabla (Composición interna)

Resuelva todas las consultas utilizando la sintaxis de SQL1 y SQL2. Las consultas con sintaxis de SQL2 se deben resolver con INNER JOIN y NATURAL JOIN.

1. Obtén un listado con el nombre de cada cliente y el nombre y apellido de su representante de ventas.

   ```mysql
   SELECT c.nombre_cliente, CONCAT(e.nombre_empleado,' ', e.apellido1_empleado,' ', ifnull(e.apellido2_empleado,'')) AS representante_ventas
   FROM cliente AS c
   INNER JOIN empleado AS e ON c.codigo_rep_ventas = e.codigo_empleado;
   +--------------------------------+--------------------------------+
   | nombre_cliente                 | representante_ventas           |
   +--------------------------------+--------------------------------+
   | GoldFish Garden                | Felipe Rosas Marquez           |
   | Gardening Associates           | Juan Carlos Ortiz Serrano      |
   | Gerudo Valley                  | Lucio Campoamor Martín         |
   | Tendo Garden                   | Mariano López Murcia           |
   | Lasas S.A.                     | José Manuel Martinez De la Osa |
   | Beragua                        | David Palma Aceituno           |
   | Club Golf Puerta del hierro    | Oscar Palma Aceituno           |
   | Naturagua                      | Lionel Narvaez                 |
   | DaraDistribuciones             | Lionel Narvaez                 |
   | Madrileña de riegos            | Laurent Serra                  |
   | Lasas S.A.                     | Walter Santiago Sanchez Lopez  |
   | Camunas Jardines S.L.          | Marcus Paxton                  |
   | Dardena S.A.                   | Lorena Paxton                  |
   | Jardin de Flores               | Narumi Riko                    |
   | Flores Marivi                  | Narumi Riko                    |
   | Flowers, S.A                   | Takuma Nomura                  |
   | Naturajardin                   | Larry Westfalls                |
   | Golf S.A.                      | John Walton                    |
   | Americh Golf Management SL     | Julian Bellinelli              |
   | Aloha                          | Mariko Kishi                   |
   | El Prat                        | Hilario Rodriguez Huertas      |
   | Sotogrande                     | Lionel Narvaez                 |
   | Vivero Humanes                 | Narumi Riko                    |
   | Fuenla City                    | Narumi Riko                    |
   | Jardines y Mansiones Cactus SL | Takuma Nomura                  |
   | Jardinerías Matías SL          | Narumi Riko                    |
   | Agrojardin                     | Julian Bellinelli              |
   | Top Campo                      | Mariko Kishi                   |
   | Jardineria Sara                | Hilario Rodriguez Huertas      |
   | Campohermoso                   | Felipe Rosas Marquez           |
   | france telecom                 | Juan Carlos Ortiz Serrano      |
   | Musée du Louvre                | Lucio Campoamor Martín         |
   | Tutifruti S.A                  | Narumi Riko                    |
   | Flores S.L.                    | José Manuel Martinez De la Osa |
   | The Magic Garden               | Mariano López Murcia           |
   | El Jardin Viviente S.L         | Narumi Riko                    |
   +--------------------------------+--------------------------------+
   36 rows in set (0.00 sec)
   ```

   

2. Muestra el nombre de los clientes que hayan realizado pagos junto con el nombre de sus representantes de ventas.

   ```mysql
   SELECT DISTINCT c.nombre_cliente, CONCAT(e.nombre_empleado,' ', e.apellido1_empleado,' ', ifnull(e.apellido2_empleado,'')) AS representante_ventas
   FROM cliente AS c
   INNER JOIN empleado AS e ON c.codigo_rep_ventas = e.codigo_empleado
   INNER JOIN pago AS p ON c.codigo_cliente = p.codigo_cliente;
   +--------------------------------+---------------------------+
   | nombre_cliente                 | representante_ventas      |
   +--------------------------------+---------------------------+
   | GoldFish Garden                | Felipe Rosas Marquez      |
   | Gardening Associates           | Juan Carlos Ortiz Serrano |
   | Gerudo Valley                  | Lucio Campoamor Martín    |
   | Tendo Garden                   | Mariano López Murcia      |
   | Beragua                        | David Palma Aceituno      |
   | Naturagua                      | Lionel Narvaez            |
   | Camunas Jardines S.L.          | Marcus Paxton             |
   | Dardena S.A.                   | Lorena Paxton             |
   | Jardin de Flores               | Narumi Riko               |
   | Flores Marivi                  | Narumi Riko               |
   | Golf S.A.                      | John Walton               |
   | Sotogrande                     | Lionel Narvaez            |
   | Jardines y Mansiones Cactus SL | Takuma Nomura             |
   | Jardinerías Matías SL          | Narumi Riko               |
   | Agrojardin                     | Julian Bellinelli         |
   | Jardineria Sara                | Hilario Rodriguez Huertas |
   | Tutifruti S.A                  | Narumi Riko               |
   | El Jardin Viviente S.L         | Narumi Riko               |
   +--------------------------------+---------------------------+
   18 rows in set (0.00 sec)
   ```

   

3. Muestra el nombre de los clientes que no hayan realizado pagos junto con el nombre de sus representantes de ventas.

   ```mysql
   SELECT c.nombre_cliente, CONCAT(e.nombre_empleado,' ', e.apellido1_empleado,' ', ifnull(e.apellido2_empleado,'')) AS representante_ventas
   FROM cliente AS c
   INNER JOIN empleado AS e ON c.codigo_rep_ventas = e.codigo_empleado
   LEFT JOIN pago AS p ON c.codigo_cliente = p.codigo_cliente
   WHERE p.codigo_pago IS NULL;
   +-----------------------------+--------------------------------+
   | nombre_cliente              | representante_ventas           |
   +-----------------------------+--------------------------------+
   | Lasas S.A.                  | José Manuel Martinez De la Osa |
   | Club Golf Puerta del hierro | Oscar Palma Aceituno           |
   | DaraDistribuciones          | Lionel Narvaez                 |
   | Madrileña de riegos         | Laurent Serra                  |
   | Lasas S.A.                  | Walter Santiago Sanchez Lopez  |
   | Flowers, S.A                | Takuma Nomura                  |
   | Naturajardin                | Larry Westfalls                |
   | Americh Golf Management SL  | Julian Bellinelli              |
   | Aloha                       | Mariko Kishi                   |
   | El Prat                     | Hilario Rodriguez Huertas      |
   | Vivero Humanes              | Narumi Riko                    |
   | Fuenla City                 | Narumi Riko                    |
   | Top Campo                   | Mariko Kishi                   |
   | Campohermoso                | Felipe Rosas Marquez           |
   | france telecom              | Juan Carlos Ortiz Serrano      |
   | Musée du Louvre             | Lucio Campoamor Martín         |
   | Flores S.L.                 | José Manuel Martinez De la Osa |
   | The Magic Garden            | Mariano López Murcia           |
   +-----------------------------+--------------------------------+
   18 rows in set (0.00 sec)
   ```

   

4. Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

   ```mysql
   SELECT DISTINCT cli.nombre_cliente, CONCAT(e.nombre_empleado,' ', e.apellido1_empleado,' ', ifnull(e.apellido2_empleado,'')) AS representante_ventas, c.nombre_ciudad
   FROM cliente AS cli
   INNER JOIN empleado as e ON cli.codigo_rep_ventas = e.codigo_empleado
   INNER JOIN pago AS p ON cli.codigo_cliente = p.codigo_cliente
   INNER JOIN oficina AS o ON e.codigo_oficina = o.codigo_oficina
   INNER JOIN ciudad as c ON o.codigo_ciudad = c.codigo_ciudad;
   +--------------------------------+---------------------------+----------------------+
   | nombre_cliente                 | representante_ventas      | nombre_ciudad        |
   +--------------------------------+---------------------------+----------------------+
   | GoldFish Garden                | Felipe Rosas Marquez      | Talavera de la Reina |
   | Gardening Associates           | Juan Carlos Ortiz Serrano | Talavera de la Reina |
   | Gerudo Valley                  | Lucio Campoamor Martín    | Madrid               |
   | Tendo Garden                   | Mariano López Murcia      | Madrid               |
   | Beragua                        | David Palma Aceituno      | Barcelona            |
   | Naturagua                      | Lionel Narvaez            | Paris                |
   | Camunas Jardines S.L.          | Marcus Paxton             | Boston               |
   | Dardena S.A.                   | Lorena Paxton             | Boston               |
   | Jardin de Flores               | Narumi Riko               | Tokyo                |
   | Flores Marivi                  | Narumi Riko               | Tokyo                |
   | Golf S.A.                      | John Walton               | Londres              |
   | Sotogrande                     | Lionel Narvaez            | Paris                |
   | Jardines y Mansiones Cactus SL | Takuma Nomura             | Tokyo                |
   | Jardinerías Matías SL          | Narumi Riko               | Tokyo                |
   | Agrojardin                     | Julian Bellinelli         | Sydney               |
   | Jardineria Sara                | Hilario Rodriguez Huertas | Madrid               |
   | Tutifruti S.A                  | Narumi Riko               | Tokyo                |
   | El Jardin Viviente S.L         | Narumi Riko               | Tokyo                |
   +--------------------------------+---------------------------+----------------------+
   18 rows in set (0.00 sec)
   ```

   

5. Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

   ```mysql
   SELECT c.nombre_cliente, CONCAT(e.nombre_empleado,' ', e.apellido1_empleado,' ', ifnull(e.apellido2_empleado,'')) AS representante_ventas, ci.nombre_ciudad
   FROM cliente AS c
   INNER JOIN empleado as e ON c.codigo_rep_ventas = e.codigo_empleado
   LEFT JOIN pago as p ON c.codigo_cliente = p.codigo_cliente
   INNER JOIN oficina as o ON e.codigo_oficina = o.codigo_oficina
   INNER JOIN ciudad AS ci ON o.codigo_ciudad = ci.codigo_ciudad
   WHERE p.codigo_pago IS NULL;
   +-----------------------------+--------------------------------+----------------------+
   | nombre_cliente              | representante_ventas           | nombre_ciudad        |
   +-----------------------------+--------------------------------+----------------------+
   | Lasas S.A.                  | José Manuel Martinez De la Osa | Barcelona            |
   | Club Golf Puerta del hierro | Oscar Palma Aceituno           | Barcelona            |
   | DaraDistribuciones          | Lionel Narvaez                 | Paris                |
   | Madrileña de riegos         | Laurent Serra                  | Paris                |
   | Lasas S.A.                  | Walter Santiago Sanchez Lopez  | San Francisco        |
   | Flowers, S.A                | Takuma Nomura                  | Tokyo                |
   | Naturajardin                | Larry Westfalls                | Londres              |
   | Americh Golf Management SL  | Julian Bellinelli              | Sydney
             |
   | Aloha                       | Mariko Kishi                   | Sydney               |
   | El Prat                     | Hilario Rodriguez Huertas      | Madrid               |
   | Vivero Humanes              | Narumi Riko                    | Tokyo                |
   | Fuenla City                 | Narumi Riko                    | Tokyo                |
   | Top Campo                   | Mariko Kishi                   | Sydney               |
   | Campohermoso                | Felipe Rosas Marquez           | Talavera de la Reina |
   | france telecom              | Juan Carlos Ortiz Serrano      | Talavera de la Reina |
   | Musée du Louvre             | Lucio Campoamor Martín         | Madrid               |
   | Flores S.L.                 | José Manuel Martinez De la Osa | Barcelona            |
   | The Magic Garden            | Mariano López Murcia           | Madrid               |
   +-----------------------------+--------------------------------+----------------------+
   18 rows in set (0.00 sec)
   ```

   

6. Lista la dirección de las oficinas que tengan clientes en Fuenlabrada.

   ```mysql
   SELECT d.nombre_calle, d.numero_direccion, d.nombre_barrio
   FROM direccion AS d
   INNER JOIN oficina as o ON o.codigo_oficina = o.codigo_oficina
   INNER JOIN ciudad as c ON o.codigo_ciudad = c.codigo_ciudad
   INNER JOIN cliente AS cli ON cli.codigo_ciudad = c.codigo_ciudad
   WHERE c.nombre_ciudad = 'Fuenlabrada';
   Empty set (0,01 sec)
   
   ```

   

7. Devuelve el nombre de los clientes y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

   ```mysql
   SELECT c.nombre_cliente, CONCAT(e.nombre_empleado,' ', e.apellido1_empleado,' ', ifnull(e.apellido2_empleado,'')) AS representante_ventas, ci.nombre_ciudad
   FROM cliente c
   INNER JOIN empleado e ON c.codigo_rep_ventas = e.codigo_empleado
   INNER JOIN oficina o ON e.codigo_oficina = o.codigo_oficina
   INNER JOIN ciudad ci ON o.codigo_ciudad = ci.codigo_ciudad;
   +--------------------------------+--------------------------------+----------------------+
   | nombre_cliente                 | representante_ventas           | nombre_ciudad        |
   +--------------------------------+--------------------------------+----------------------+
   | Lasas S.A.                     | José Manuel Martinez De la Osa | Barcelona            |
   | Flores S.L.                    | José Manuel Martinez De la Osa | Barcelona            |
   | Beragua                        | David Palma Aceituno           | Barcelona            |
   | Club Golf Puerta del hierro    | Oscar Palma Aceituno           | Barcelona            |
   | Camunas Jardines S.L.          | Marcus Paxton                  | Boston               |
   | Dardena S.A.                   | Lorena Paxton                  | Boston               |
   | Naturajardin                   | Larry Westfalls                | Londres              |
   | Golf S.A.                      | John Walton                    | Londres              |
   | Tendo Garden                   | Mariano López Murcia           | Madrid               |
   | The Magic Garden               | Mariano López Murcia           | Madrid               |
   | Gerudo Valley                  | Lucio Campoamor Martín         | Madrid               |
   | Musée du Louvre                | Lucio Campoamor Martín         | Madrid               |
   | El Prat                        | Hilario Rodriguez Huertas      | Madrid               |
   | Jardineria Sara                | Hilario Rodriguez Huertas      | Madrid               |
   | Naturagua                      | Lionel Narvaez                 | Paris                |
   | DaraDistribuciones             | Lionel Narvaez                 | Paris                |
   | Sotogrande                     | Lionel Narvaez                 | Paris                |
   | Madrileña de riegos            | Laurent Serra                  | Paris                |
   | Lasas S.A.                     | Walter Santiago Sanchez Lopez  | San Francisco        |
   | Americh Golf Management SL     | Julian Bellinelli              | Sydney               |
   | Agrojardin                     | Julian Bellinelli              | Sydney               |
   | Aloha                          | Mariko Kishi                   | Sydney               |
   | Top Campo                      | Mariko Kishi                   | Sydney               |
   | GoldFish Garden                | Felipe Rosas Marquez           | Talavera de la Reina |
   | Campohermoso                   | Felipe Rosas Marquez           | Talavera de la Reina |
   | Gardening Associates           | Juan Carlos Ortiz Serrano      | Talavera de la Reina |
   | france telecom                 | Juan Carlos Ortiz Serrano      | Talavera de la Reina |
   | Jardin de Flores               | Narumi Riko                    | Tokyo                |
   | Flores Marivi                  | Narumi Riko                    | Tokyo                |
   | Vivero Humanes                 | Narumi Riko                    | Tokyo
                |
   | Fuenla City                    | Narumi Riko                    | Tokyo                |
   | Jardinerías Matías SL          | Narumi Riko                    | Tokyo                |
   | Tutifruti S.A                  | Narumi Riko                    | Tokyo                |
   | El Jardin Viviente S.L         | Narumi Riko                    | Tokyo                |
   | Flowers, S.A                   | Takuma Nomura                  | Tokyo                |
   | Jardines y Mansiones Cactus SL | Takuma Nomura                  | Tokyo                |
   +--------------------------------+--------------------------------+----------------------+
   36 rows in set (0.00 sec)
   ```

   

8. Devuelve un listado con el nombre de los empleados junto con el nombre de sus jefes.

   ```mysql
   SELECT CONCAT(e.nombre_empleado,' ', e.apellido1_empleado,' ', ifnull(e.apellido2_empleado,'')) AS empleado, CONCAT(j.nombre_empleado,' ', j.apellido1_empleado,' ', ifnull(j.apellido2_empleado,'')) AS nombre_jefe
   FROM empleado e
   INNER JOIN empleado j ON e.codigo_jefe = j.codigo_empleado;
   +--------------------------------+------------------------+
   | empleado                       | nombre_jefe            |
   +--------------------------------+------------------------+
   | Ruben López Martinez           | Marcos Magaña Perez    |
   | Alberto Soria Carrasco         | Ruben López Martinez   |
   | Maria Solís Jerez              | Ruben López Martinez   |
   | Felipe Rosas Marquez           | Alberto Soria Carrasco |
   | Juan Carlos Ortiz Serrano      | Alberto Soria Carrasco |
   | Carlos Soria Jimenez           | Alberto Soria Carrasco |
   | Mariano López Murcia           | Carlos Soria Jimenez   |
   | Lucio Campoamor Martín         | Carlos Soria Jimenez   |
   | Hilario Rodriguez Huertas      | Carlos Soria Jimenez   |
   | Emmanuel Magaña Perez          | Alberto Soria Carrasco |
   | José Manuel Martinez De la Osa | Emmanuel Magaña Perez  |
   | David Palma Aceituno           | Emmanuel Magaña Perez  |
   | Oscar Palma Aceituno           | Emmanuel Magaña Perez  |
   | Francois Fignon                | Alberto Soria Carrasco |
   | Lionel Narvaez                 | Francois Fignon        |
   | Laurent Serra                  | Francois Fignon        |
   | Michael Bolton                 | Alberto Soria Carrasco |
   | Walter Santiago Sanchez Lopez  | Michael Bolton         |
   | Hilary Washington              | Alberto Soria Carrasco |
   | Marcus Paxton                  | Hilary Washington      |
   | Lorena Paxton                  | Hilary Washington      |
   | Nei Nishikori                  | Alberto Soria Carrasco |
   | Narumi Riko                    | Nei Nishikori          |
   | Takuma Nomura                  | Nei Nishikori          |
   | Amy Johnson                    | Alberto Soria Carrasco |
   | Larry Westfalls                | Amy Johnson            |
   | John Walton                    | Amy Johnson            |
   | Kevin Fallmer                  | Alberto Soria Carrasco |
   | Julian Bellinelli              | Kevin Fallmer          |
   | Mariko Kishi                   | Kevin Fallmer          |
   +--------------------------------+------------------------+
   30 rows in set (0.00 sec)
   
   ```

   

9. Devuelve un listado que muestre el nombre de cada empleados, el nombre de su jefe y el nombre del jefe de sus jefe.

   ```mysql
   SELECT CONCAT(e.nombre_empleado,' ', e.apellido1_empleado,' ', ifnull(e.apellido2_empleado,'')) AS empleado, CONCAT(j1.nombre_empleado,' ', j1.apellido1_empleado,' ', ifnull(j1.apellido2_empleado,'')) AS nombre_jefe, CONCAT(j2.nombre_empleado,' ', j2.apellido1_empleado,' ', ifnull(j2.apellido2_empleado,'')) AS jefe_del_jefe
   FROM empleado e
   INNER JOIN empleado j1 ON e.codigo_jefe = j1.codigo_empleado
   INNER JOIN empleado j2 ON j1.codigo_jefe = j2.codigo_empleado;
   +--------------------------------+------------------------+------------------------+
   | empleado                       | nombre_jefe            | jefe_del_jefe          |
   +--------------------------------+------------------------+------------------------+
   | Alberto Soria Carrasco         | Ruben López Martinez   | Marcos Magaña Perez    |
   | Maria Solís Jerez              | Ruben López Martinez   | Marcos Magaña Perez    |
   | Felipe Rosas Marquez           | Alberto Soria Carrasco | Ruben López Martinez   |
   | Juan Carlos Ortiz Serrano      | Alberto Soria Carrasco | Ruben López Martinez   |
   | Carlos Soria Jimenez           | Alberto Soria Carrasco | Ruben López Martinez   |
   | Mariano López Murcia           | Carlos Soria Jimenez   | Alberto Soria Carrasco |
   | Lucio Campoamor Martín         | Carlos Soria Jimenez   | Alberto Soria Carrasco |
   | Hilario Rodriguez Huertas      | Carlos Soria Jimenez   | Alberto Soria Carrasco |
   | Emmanuel Magaña Perez          | Alberto Soria Carrasco | Ruben López Martinez   |
   | José Manuel Martinez De la Osa | Emmanuel Magaña Perez  | Alberto Soria Carrasco |
   | David Palma Aceituno           | Emmanuel Magaña Perez  | Alberto Soria Carrasco |
   | Oscar Palma Aceituno           | Emmanuel Magaña Perez  | Alberto Soria Carrasco |
   | Francois Fignon                | Alberto Soria Carrasco | Ruben López Martinez   |
   | Lionel Narvaez                 | Francois Fignon        | Alberto Soria Carrasco |
   | Laurent Serra                  | Francois Fignon        | Alberto Soria Carrasco |
   | Michael Bolton                 | Alberto Soria Carrasco | Ruben López Martinez   |
   | Walter Santiago Sanchez Lopez  | Michael Bolton         | Alberto Soria Carrasco |
   | Hilary Washington              | Alberto Soria Carrasco | Ruben López Martinez   |
   | Marcus Paxton                  | Hilary Washington      | Alberto Soria Carrasco |
   | Lorena Paxton                  | Hilary Washington      | Alberto Soria Carrasco |
   | Nei Nishikori                  | Alberto Soria Carrasco | Ruben López Martinez   |
   | Narumi Riko                    | Nei Nishikori          | Alberto Soria Carrasco |
   | Takuma Nomura                  | Nei Nishikori          | Alberto Soria Carrasco |
   | Amy Johnson                    | Alberto Soria Carrasco | Ruben López Martinez   |
   | Larry Westfalls                | Amy Johnson            | Alberto Soria Carrasco |
   | John Walton                    | Amy Johnson            | Alberto Soria Carrasco |
   | Kevin Fallmer                  | Alberto Soria Carrasco | Ruben López Martinez   |
   | Julian Bellinelli              | Kevin Fallmer          | Alberto Soria Carrasco |
   | Mariko Kishi                   | Kevin Fallmer          | Alberto Soria Carrasco |
   +--------------------------------+------------------------+------------------------+
   29 rows in set (0.00 sec)
   ```

   

10. Devuelve el nombre de los clientes a los que no se les ha entregado a tiempo un pedido.

    ```mysql
    SELECT c.nombre_cliente
    FROM cliente c
    INNER JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
    WHERE p.fecha_entrega > p.fecha_esperada;
    Empty set (0,00 sec)
    ```

    

11. Devuelve un listado de las diferentes gamas de producto que ha comprado cada cliente.

    ```mysql
    SELECT DISTINCT c.nombre_cliente, gp.gama
    FROM cliente c
    INNER JOIN pedido pe ON c.codigo_cliente = pe.codigo_cliente
    INNER JOIN detalle_pedido dp ON pe.codigo_pedido = dp.codigo_pedido
    INNER JOIN producto p ON dp.codigo_producto = p.codigo_producto
    INNER JOIN gama_producto gp ON p.codigo_gama = gp.codigo_gama;
    +--------------------------------+--------------+
    | nombre_cliente                 | gama         |
    +--------------------------------+--------------+
    | GoldFish Garden                | Herbaceas    |
    | Lasas S.A.                     | Herbaceas    |
    | Beragua                        | Herbaceas    |
    | Club Golf Puerta del hierro    | Herbaceas    |
    | Naturagua                      | Herbaceas    |
    | Beragua                        | Herramientas |
    | DaraDistribuciones             | Herramientas |
    | Madrileña de riegos            | Herramientas |
    | Lasas S.A.                     | Herramientas |
    | Jardines y Mansiones Cactus SL | Herramientas |
    | france telecom                 | Herramientas |
    | Top Campo                      | Herramientas |
    | Gardening Associates           | Aromáticas   |
    | Camunas Jardines S.L.          | Aromáticas   |
    | Dardena S.A.                   | Aromáticas   |
    | Jardin de Flores               | Aromáticas   |
    | Gerudo Valley                  | Frutales     |
    | Flores Marivi                  | Frutales     |
    | Flowers, S.A                   | Frutales     |
    | Naturajardin                   | Frutales     |
    | El Prat                        | Frutales     |
    | Sotogrande                     | Frutales     |
    | Fuenla City                    | Frutales     |
    | Campohermoso                   | Frutales     |
    | Tendo Garden                   | Ornamentales |
    | Golf S.A.                      | Ornamentales |
    | Americh Golf Management SL     | Ornamentales |
    | Aloha                          | Ornamentales |
    | Vivero Humanes                 | Ornamentales |
    | Jardinerías Matías SL          | Ornamentales |
    | Musée du Louvre                | Ornamentales |
    | Agrojardin                     | Ornamentales |
    | Jardineria Sara                | Ornamentales |
    +--------------------------------+--------------+
    33 rows in set (0.00 sec)
    ```

    

### Consultas multitabla (Composición externa)

Resuelva todas las consultas utilizando las cláusulas LEFT JOIN, RIGHT JOIN, NATURAL LEFT JOIN y NATURAL RIGHT JOIN.

1. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.

   ```mysql
   SELECT c.codigo_cliente, c.nombre_cliente
   FROM cliente AS c
   LEFT JOIN pago AS p ON c.codigo_cliente = p.codigo_cliente
   WHERE p.codigo_pago IS NULL;
   +----------------+-----------------------------+
   | codigo_cliente | nombre_cliente              |
   +----------------+-----------------------------+
   |              6 | Lasas S.A.                  |
   |              8 | Club Golf Puerta del hierro |
   |             10 | DaraDistribuciones          |
   |             11 | Madrileña de riegos         |
   |             12 | Lasas S.A.                  |
   |             17 | Flowers, S.A                |
   |             18 | Naturajardin                |
   |             20 | Americh Golf Management SL  |
   |             21 | Aloha                       |
   |             22 | El Prat                     |
   |             24 | Vivero Humanes              |
   |             25 | Fuenla City                 |
   |             29 | Top Campo                   |
   |             31 | Campohermoso                |
   |             32 | france telecom              |
   |             33 | Musée du Louvre             |
   |             36 | Flores S.L.                 |
   |             37 | The Magic Garden            |
   +----------------+-----------------------------+
   18 rows in set (0.00 sec)
   ```

   

2. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pedido.

   ```mysql
   SELECT c.codigo_cliente, c.nombre_cliente
   FROM cliente AS c
   NATURAL LEFT JOIN pedido AS p
   WHERE p.codigo_pedido IS NULL;
   Empty set (0,00 sec)
   ```

   

3. Devuelve un listado que muestre los clientes que no han realizado ningún pago y los que no han realizado ningún pedido.

   ```mysql
   SELECT c.codigo_cliente, c.nombre_cliente
   FROM cliente AS c
   NATURAL LEFT JOIN pago AS p
   WHERE p.codigo_pago IS NULL
   UNION
   SELECT c.codigo_cliente, c.nombre_cliente
   FROM cliente AS c
   NATURAL LEFT JOIN pedido
   WHERE pedido.codigo_pedido IS NULL;
   +----------------+-----------------------------+
   | codigo_cliente | nombre_cliente              |
   +----------------+-----------------------------+
   |              6 | Lasas S.A.                  |
   |              8 | Club Golf Puerta del hierro |
   |             10 | DaraDistribuciones          |
   |             11 | Madrileña de riegos         |
   |             12 | Lasas S.A.                  |
   |             17 | Flowers, S.A                |
   |             18 | Naturajardin                |
   |             20 | Americh Golf Management SL  |
   |             21 | Aloha                       |
   |             22 | El Prat                     |
   |             24 | Vivero Humanes              |
   |             25 | Fuenla City                 |
   |             29 | Top Campo                   |
   |             31 | Campohermoso                |
   |             32 | france telecom              |
   |             33 | Musée du Louvre             |
   |             36 | Flores S.L.                 |
   |             37 | The Magic Garden            |
   +----------------+-----------------------------+
   18 rows in set (0.00 sec)
   ```

   

4. Devuelve un listado que muestre solamente los empleados que no tienen una oficina asociada.

   ```mysql
   SELECT e.codigo_empleado, CONCAT(e.nombre_empleado,' ', e.apellido1_empleado,' ', ifnull(e.apellido2_empleado,'')) AS nombre_empleado, e.email_empleado, e.codigo_oficina, e.codigo_jefe, e.codigo_puesto_empleado
   FROM empleado e
   NATURAL LEFT JOIN oficina o
   WHERE o.codigo_oficina IS NULL;
   Empty set (0,00 sec)
   ```

   

5. Devuelve un listado que muestre solamente los empleados que no tienen un cliente asociado.

   ```mysql
   SELECT e.codigo_empleado, CONCAT(e.nombre_empleado,' ', e.apellido1_empleado,' ', ifnull(e.apellido2_empleado,'')) AS nombre_empleado, e.email_empleado, e.codigo_oficina, e.codigo_jefe, e.codigo_puesto_empleado
   FROM empleado e
   NATURAL LEFT JOIN cliente c
   WHERE c.codigo_cliente IS NULL;
   Empty set (0,00 sec)
   ```

   

6. Devuelve un listado que muestre solamente los empleados que no tienen un cliente asociado junto con los datos de la oficina donde trabajan.

   ```mysql
   SELECT e.codigo_empleado, CONCAT(e.nombre_empleado,' ', e.apellido1_empleado,' ', ifnull(e.apellido2_empleado,'')) AS nombre_empleado, e.email_empleado, e.codigo_oficina, e.codigo_jefe, e.codigo_puesto_empleado
   FROM empleado e
   NATURAL LEFT JOIN cliente c
   LEFT JOIN oficina o ON e.codigo_oficina = o.codigo_oficina
   WHERE c.codigo_cliente IS NULL;
   Empty set (0,00 sec)
   ```

   

7. Devuelve un listado que muestre los empleados que no tienen una oficina asociada y los que no tienen un cliente asociado.

   ```mysql
   SELECT e.codigo_empleado, CONCAT(e.nombre_empleado,' ', e.apellido1_empleado,' ', ifnull(e.apellido2_empleado,'')) AS nombre_empleado, e.email_empleado, e.codigo_oficina, e.codigo_jefe, e.codigo_puesto_empleado
   FROM empleado e
   LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_rep_ventas
   WHERE c.codigo_cliente IS NULL;
   +-----------------+------------------------+---------------------------+----------------+-------------+------------------------+
   | codigo_empleado | nombre_empleado        | email_empleado            | codigo_oficina | codigo_jefe | codigo_puesto_empleado |
   +-----------------+------------------------+---------------------------+----------------+-------------+------------------------+
   |               1 | Marcos Magaña Perez    | marcos@jardineria.es      |              8 |        NULL |                      1 |
   |               2 | Ruben López Martinez   | rlopez@jardineria.es      |              8 |           1 |                      2 |
   |               3 | Alberto Soria Carrasco | asoria@jardineria.es      |              8 |           2 |                      3 |
   |               4 | Maria Solís Jerez      | msolis@jardineria.es      |              8 |           2 |                      4 |
   |               7 | Carlos Soria Jimenez   | csoria@jardineria.es      |              4 |           3 |                      6 |
   |              11 | Emmanuel Magaña Perez  | manu@jardineria.es        |              1 |           3 |                      6 |
   |              15 | Francois Fignon        | ffignon@gardening.com     |              5 |           3 |                      6 |
   |              18 | Michael Bolton         | mbolton@gardening.com     |              6 |           3 |                      6 |
   |              20 | Hilary Washington      | hwashington@gardening.com |              2 |           3 |                      6 |
   |              23 | Nei Nishikori          | nnishikori@gardening.com  |              9 |           3 |                      6 |
   |              26 | Amy Johnson            | ajohnson@gardening.com    |              3 |           3 |                      6 |
   |              29 | Kevin Fallmer          | kfalmer@gardening.com     |              7 |           3 |                      6 |
   +-----------------+------------------------+---------------------------+----------------+-------------+------------------------+
   12 rows in set (0.00 sec)
   ```

   

8. Devuelve un listado de los productos que nunca han aparecido en un pedido.

   ```mysql
   SELECT p.codigo_producto, p.nombre, p.codigo_gama, p.cantidad_stock, p.precio_venta, p.codigo_dimensiones
   FROM producto p
   LEFT JOIN detalle_pedido dp ON p.codigo_producto = dp.codigo_producto
   WHERE dp.codigo_producto IS NULL;
   Empty set (0.00 sec)
   ```

   

9. Devuelve un listado de los productos que nunca han aparecido en un pedido. El resultado debe mostrar el nombre, la descripción y la imagen del producto.

   ```mysql
   
   ```

   

10. Devuelve las oficinas donde no trabajan ninguno de los empleados que hayan sido los representantes de ventas de algún cliente que haya realizado la compra de algún producto de la gama Frutales.

    ```mysql
    SELECT o.codigo_oficina, o.codigo_ciudad, o.codigo_postal
    FROM oficina o
    LEFT JOIN empleado e ON o.codigo_oficina = e.codigo_oficina
    LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_rep_ventas
    LEFT JOIN pedido ped ON c.codigo_cliente = ped.codigo_cliente
    LEFT JOIN detalle_pedido dp ON ped.codigo_pedido = dp.codigo_pedido
    LEFT JOIN producto prod ON dp.codigo_producto = prod.codigo_producto
    LEFT JOIN gama_producto gp ON prod.codigo_gama = gp.codigo_gama
    WHERE gp.gama = 'Frutales'
    GROUP BY o.codigo_oficina
    HAVING COUNT(e.codigo_empleado) = 0;
    Empty set (0,00 sec)
    ```

    

11. Devuelve un listado con los clientes que han realizado algún pedido pero no han realizado ningún pago.

    ```mysql
    SELECT codigo_cliente, nombre_cliente, codigo_ciudad, codigo_postal, limite_credito, codigo_rep_ventas
    FROM cliente
    WHERE codigo_cliente IN (
        SELECT DISTINCT codigo_cliente
        FROM pedido
        WHERE codigo_cliente NOT IN (
            SELECT DISTINCT codigo_cliente
            FROM pago
        )
    );
    +----------------+-----------------------------+---------------+---------------+----------------+-------------------+
    | codigo_cliente | nombre_cliente              | codigo_ciudad | codigo_postal | limite_credito | codigo_rep_ventas |
    +----------------+-----------------------------+---------------+---------------+----------------+-------------------+
    |              6 | Lasas S.A.                  |             2 | 28945         |      154310.00 |                12 |
    |              8 | Club Golf Puerta del hierro |             3 | 28930         |       40000.00 |                14 |
    |             10 | DaraDistribuciones          |             9 | 28946         |       50000.00 |                16 |
    |             11 | Madrileña de riegos         |             8 | 28943         |       20000.00 |                17 |
    |             12 | Lasas S.A.                  |             5 | 28945         |       15431.00 |                19 |
    |             17 | Flowers, S.A                |             2 | 24586         |        3500.00 |                25 |
    |             18 | Naturajardin                |             1 | 28011         |        5050.00 |                27 |
    |             20 | Americh Golf Management SL  |             8 | 12320         |       20000.00 |                30 |
    |             21 | Aloha                       |             6 | 35488         |       50000.00 |                31 |
    |             22 | El Prat                     |             5 | 12320         |       30000.00 |                10 |
    |             24 | Vivero Humanes              |             1 | 28970         |        7430.00 |                24 |
    |             25 | Fuenla City                 |             7 | 28574         |        4500.00 |                24 |
    |             29 | Top Campo                   |             2 | 28574         |        5500.00 |                31 |
    |             31 | Campohermoso                |             8 | 28945         |        3250.00 |                 5 |
    |             32 | france telecom              |             6 | 75010         |       10000.00 |                 6 |
    |             33 | Musée du Louvre             |             2 | 75058         |       30000.00 |                 9 |
    |             36 | Flores S.L.                 |             9 | 29643         |        6000.00 |                12 |
    |             37 | The Magic Garden            |             8 | 65930         |       10000.00 |                 8 |
    +----------------+-----------------------------+---------------+---------------+----------------+-------------------+
    18 rows in set (0.00 sec)
    ```

    

12. Devuelve un listado con los datos de los empleados que no tienen clientes asociados y el nombre de su jefe asociado.

    ```mysql
    SELECT CONCAT(e.nombre_empleado,' ', e.apellido1_empleado,' ', ifnull(e.apellido2_empleado,'')) AS nombre_empleado, e.email_empleado, e.codigo_jefe, CONCAT(j.nombre_empleado,' ', j.apellido1_empleado,' ', ifnull(j.apellido2_empleado,'')) AS nombre_jefe
    FROM empleado e
    LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_rep_ventas
    LEFT JOIN empleado j ON e.codigo_jefe = j.codigo_empleado
    WHERE c.codigo_rep_ventas IS NULL;
    +------------------------+---------------------------+-------------+------------------------+
    | nombre_empleado        | email_empleado            | codigo_jefe | nombre_jefe            |
    +------------------------+---------------------------+-------------+------------------------+
    | Marcos Magaña Perez    | marcos@jardineria.es      |        NULL | NULL                   |
    | Ruben López Martinez   | rlopez@jardineria.es      |           1 | Marcos Magaña Perez    |
    | Alberto Soria Carrasco | asoria@jardineria.es      |           2 | Ruben López Martinez   |
    | Maria Solís Jerez      | msolis@jardineria.es      |           2 | Ruben López Martinez   |
    | Carlos Soria Jimenez   | csoria@jardineria.es      |           3 | Alberto Soria Carrasco |
    | Emmanuel Magaña Perez  | manu@jardineria.es        |           3 | Alberto Soria Carrasco |
    | Francois Fignon        | ffignon@gardening.com     |           3 | Alberto Soria Carrasco |
    | Michael Bolton         | mbolton@gardening.com     |           3 | Alberto Soria Carrasco |
    | Hilary Washington      | hwashington@gardening.com |           3 | Alberto Soria Carrasco |
    | Nei Nishikori          | nnishikori@gardening.com  |           3 | Alberto Soria Carrasco |
    | Amy Johnson            | ajohnson@gardening.com    |           3 | Alberto Soria Carrasco |
    | Kevin Fallmer          | kfalmer@gardening.com     |           3 | Alberto Soria Carrasco |
    +------------------------+---------------------------+-------------+------------------------+
    12 rows in set (0.00 sec)
    ```

    

### Consultas resumen

1. ¿Cuántos empleados hay en la compañía?

   ```mysql
   SELECT COUNT(codigo_empleado) AS total_empleados
   FROM empleado;
   +-----------------+
   | total_empleados |
   +-----------------+
   |              31 |
   +-----------------+
   1 row in set (0.00 sec)
   
   ```

   

2. ¿Cuántos clientes tiene cada país?

   ```mysql
   SELECT p.nombre_pais, COUNT(c.codigo_cliente) AS total_clientes
   FROM cliente c
   JOIN ciudad ci ON c.codigo_ciudad = ci.codigo_ciudad
   JOIN region r ON ci.codigo_region = r.codigo_region
   JOIN pais p ON r.codigo_pais = p.codigo_pais
   GROUP BY p.nombre_pais;
   +-------------+----------------+
   | nombre_pais | total_clientes |
   +-------------+----------------+
   | Australia   |              1 |
   | España      |             12 |
   | Francia     |              5 |
   | Japón       |              4 |
   | Inglaterra  |              4 |
   | EEUU        |             10 |
   +-------------+----------------+
   6 rows in set (0.00 sec)
   ```

   

3. ¿Cuál fue el pago medio en 2009?

   ```mysql
   SELECT AVG(total_pago) AS pago_medio_2009
   FROM pago
   WHERE YEAR(fecha_pago) = 2009;
   +-----------------+
   | pago_medio_2009 |
   +-----------------+
   |     4504.076923 |
   +-----------------+
   1 row in set (0.00 sec)
   ```

   

4. ¿Cuántos pedidos hay en cada estado? Ordena el resultado de forma descendente por el número de pedidos.

   ```mysql
   SELECT e.nombre AS nombre_estado, COUNT(p.codigo_pedido) AS total_pedidos
   FROM pedido p
   JOIN estado e ON p.codigo_estado = e.codigo_estado
   GROUP BY p.codigo_estado, nombre_estado
   ORDER BY total_pedidos DESC;
   +---------------+---------------+
   | nombre_estado | total_pedidos |
   +---------------+---------------+
   | Entregado     |            14 |
   | Pendiente     |            14 |
   | Rechazado     |            12 |
   +---------------+---------------+
   3 rows in set (0.00 sec)
   ```

   

5. Calcula el precio de venta del producto más caro y más barato en una misma consulta.

   ```mysql
   SELECT MAX(precio_venta) AS precio_mas_caro, MIN(precio_venta) AS precio_mas_barato
   FROM producto;
   +-----------------+-------------------+
   | precio_mas_caro | precio_mas_barato |
   +-----------------+-------------------+
   |           60.00 |              5.99 |
   +-----------------+-------------------+
   1 row in set (0.00 sec)
   ```

   

6. Calcula el número de clientes que tiene la empresa.

   ```mysql
   SELECT COUNT(codigo_cliente) AS total_clientes
   FROM cliente;
   +----------------+
   | total_clientes |
   +----------------+
   |             36 |
   +----------------+
   1 row in set (0.00 sec)
   ```

   

7. ¿Cuántos clientes existen con domicilio en la ciudad de Madrid?

   ```mysql
   SELECT COUNT(codigo_cliente) AS total_clientes_madrid
   FROM cliente c
   JOIN ciudad ci ON c.codigo_ciudad = ci.codigo_ciudad
   WHERE ci.nombre_ciudad = 'Madrid';
   +-----------------------+
   | total_clientes_madrid |
   +-----------------------+
   |                     2 |
   +-----------------------+
   1 row in set (0,00 sec)
   ```

   

8. ¿Calcula cuántos clientes tiene cada una de las ciudades que empiezan por M?

   ```mysql
   SELECT ci.nombre_ciudad, COUNT(c.codigo_cliente) AS total_clientes
   FROM cliente c
   JOIN ciudad ci ON c.codigo_ciudad = ci.codigo_ciudad
   WHERE ci.nombre_ciudad LIKE 'M%'
   GROUP BY ci.nombre_ciudad;
   +---------------+----------------+
   | nombre_ciudad | total_clientes |
   +---------------+----------------+
   | Madrid        |              2 |
   +---------------+----------------+
   1 row in set (0,00 sec)
   ```

   

9. Devuelve el nombre de los representantes de ventas y el número de clientes al que atiende cada uno.

   ```mysql
   SELECT CONCAT(e.nombre_empleado,' ', e.apellido1_empleado,' ', ifnull(e.apellido2_empleado,'')) AS representante_ventas, COUNT(c.codigo_cliente) AS clientes_atendidos
   FROM empleado e
   JOIN cliente c ON e.codigo_empleado = c.codigo_rep_ventas
   GROUP BY representante_ventas;
   +--------------------------------+--------------------+
   | representante_ventas           | clientes_atendidos |
   +--------------------------------+--------------------+
   | Felipe Rosas Marquez           |                  2 |
   | Juan Carlos Ortiz Serrano      |                  2 |
   | Mariano López Murcia           |                  2 |
   | Lucio Campoamor Martín         |                  2 |
   | Hilario Rodriguez Huertas      |                  2 |
   | José Manuel Martinez De la Osa |                  2 |
   | David Palma Aceituno           |                  1 |
   | Oscar Palma Aceituno           |                  1 |
   | Lionel Narvaez                 |                  3 |
   | Laurent Serra                  |                  1 |
   | Walter Santiago Sanchez Lopez  |                  1 |
   | Marcus Paxton                  |                  1 |
   | Lorena Paxton                  |                  1 |
   | Narumi Riko                    |                  7 |
   | Takuma Nomura                  |                  2 |
   | Larry Westfalls                |                  1 |
   | John Walton                    |                  1 |
   | Julian Bellinelli              |                  2 |
   | Mariko Kishi                   |                  2 |
   +--------------------------------+--------------------+
   19 rows in set (0.00 sec)
   ```

   

10. Calcula el número de clientes que no tiene asignado representante de ventas.

    ```mysql
    SELECT COUNT(codigo_cliente) AS clientes_sin_representante
    FROM cliente
    WHERE codigo_rep_ventas IS NULL OR codigo_rep_ventas = '';
    +----------------------------+
    | clientes_sin_representante |
    +----------------------------+
    |                          0 |
    +----------------------------+
    1 row in set (0,00 sec)
    ```

    

11. Calcula la fecha del primer y último pago realizado por cada uno de los clientes. El listado deberá mostrar el nombre y los apellidos de cada cliente.

    ```mysql
    SELECT c.nombre_cliente, CONCAT(co.primer_nombre_contacto,' ', ifnull(co.segundo_nombre_contacto,''),' ',  co.primer_apellido_contacto,' ', ifnull(co.segundo_apellido_contacto,'')) AS nombre_contacto, MIN(p.fecha_pago) AS primera_fecha_pago, MAX(p.fecha_pago) AS ultima_fecha_pago
    FROM cliente c
    LEFT JOIN contacto co ON c.codigo_cliente = co.codigo_cliente
    LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente
    GROUP BY c.nombre_cliente, nombre_contacto;
    
    +--------------------------------+------------------+--------------------+-------------------+
    | nombre_cliente                 | nombre_contacto  | primera_fecha_pago | ultima_fecha_pago |
    +--------------------------------+------------------+--------------------+-------------------+
    | GoldFish Garden                | Daniel  G García | 2008-11-10         | 2008-12-10        |
    | Gardening Associates           | Link  F          | 2009-01-16         | 2009-02-19        |
    | Gerudo Valley                  | Akane  T         | 2007-01-08         | 2007-01-08        |
    | Tendo Garden                   | Antonio  L       | 2006-01-18         | 2006-01-18        |
    | Lasas S.A.                     | Jose  B          | NULL               | NULL              |
    | Beragua                        | Paco  L          | 2009-01-13         | 2009-01-13        |
    | Club Golf Puerta del hierro    | Guillermo  R     | NULL               | NULL              |
    | Naturagua                      | David  S         | 2009-01-06         | 2009-01-06        |
    | DaraDistribuciones             | Jose  T          | NULL               | NULL              |
    | Madrileña de riegos            | Antonio  L       | NULL               | NULL              |
    | Lasas S.A.                     | Pedro  C         | NULL               | NULL              |
    | Camunas Jardines S.L.          | Juan  R          | 2008-08-04         | 2008-08-04        |
    | Dardena S.A.                   | Javier  V        | 2008-07-15         | 2008-07-15        |
    | Jardin de Flores               | Maria  R         | 2009-01-15         | 2009-02-15        |
    | Flores Marivi                  | Beatriz  F       | 2009-02-16         | 2009-02-16        |
    | Flowers, S.A                   | Victoria  C      | NULL               | NULL              |
    | Naturajardin                   | Luis  M          | NULL               | NULL              |
    | Golf S.A.                      | Mario  S         | 2009-03-06         | 2009-03-06        |
    | Americh Golf Management SL     | Cristian  R      | NULL               | NULL              |
    | Aloha                          | Francisco  C     | NULL               | NULL              |
    | El Prat                        | Maria  S         | NULL               | NULL              |
    | Sotogrande                     | Federico  G      | 2009-03-26         | 2009-03-26        |
    | Vivero Humanes                 | Tony  M          | NULL               | NULL              |
    | Fuenla City                    | Eva María  S     | NULL               | NULL              |
    | Jardines y Mansiones Cactus SL | Matías  S        | 2008-03-18         | 2008-03-18        |
    | Jardinerías Matías SL          | Benito  L        | 2009-02-08         | 2009-02-08        |
    | Agrojardin                     | Joseluis  S      | 2009-01-13         | 2009-01-13        |
    | Top Campo                      | Sara  M          | NULL               | NULL              |
    | Jardineria Sara                | Luis  J          | 2009-01-16         | 2009-01-16        |
    | Campohermoso                   | Fraçois  T       | NULL               | NULL              |
    | france telecom                 | Pierre  D        | NULL               | NULL              |
    | Musée du Louvre                | Jacob  J         | NULL               | NULL              |
    | Tutifruti S.A                  | Richard  M       | 2007-10-06         | 2007-10-06        |
    | Flores S.L.                    | Justin  S        | NULL               | NULL              |
    | The Magic Garden               | Anne  W          | NULL               | NULL              |
    | El Jardin Viviente S.L         | Antonio  R       | 2006-05-26         | 2006-05-26        |
    +--------------------------------+------------------+--------------------+-------------------+
    36 rows in set (0.00 sec)
    ```

    

12. Calcula el número de productos diferentes que hay en cada uno de los pedidos.

    ```mysql
    SELECT codigo_pedido, COUNT(DISTINCT codigo_producto) AS num_productos_diferentes
    FROM detalle_pedido
    GROUP BY codigo_pedido;
    +---------------+--------------------------+
    | codigo_pedido | num_productos_diferentes |
    +---------------+--------------------------+
    | PED001        |                        1 |
    | PED002        |                        1 |
    | PED003        |                        1 |
    | PED004        |                        1 |
    | PED005        |                        1 |
    | PED006        |                        1 |
    | PED007        |                        1 |
    | PED008        |                        1 |
    | PED009        |                        1 |
    | PED010        |                        1 |
    | PED011        |                        1 |
    | PED012        |                        1 |
    | PED013        |                        1 |
    | PED014        |                        1 |
    | PED015        |                        1 |
    | PED016        |                        1 |
    | PED017        |                        1 |
    | PED018        |                        1 |
    | PED019        |                        1 |
    | PED020        |                        1 |
    | PED021        |                        1 |
    | PED022        |                        1 |
    | PED023        |                        1 |
    | PED024        |                        1 |
    | PED025        |                        1 |
    | PED026        |                        1 |
    | PED027        |                        1 |
    | PED028        |                        1 |
    | PED029        |                        1 |
    | PED030        |                        1 |
    | PED031        |                        1 |
    | PED032        |                        1 |
    | PED033        |                        1 |
    +---------------+--------------------------+
    33 rows in set (0.00 sec)
    ```

    

13. Calcula la suma de la cantidad total de todos los productos que aparecen en cada uno de los pedidos.

    ```mysql
    SELECT codigo_pedido, SUM(cantidad) AS cantidad_total
    FROM detalle_pedido
    GROUP BY codigo_pedido;
    +---------------+----------------+
    | codigo_pedido | cantidad_total |
    +---------------+----------------+
    | PED001        |              5 |
    | PED002        |              3 |
    | PED003        |              2 |
    | PED004        |              4 |
    | PED005        |              6 |
    | PED006        |              1 |
    | PED007        |              2 |
    | PED008        |              3 |
    | PED009        |              5 |
    | PED010        |              2 |
    | PED011        |              3 |
    | PED012        |              4 |
    | PED013        |              2 |
    | PED014        |              1 |
    | PED015        |              3 |
    | PED016        |              4 |
    | PED017        |              5 |
    | PED018        |              3 |
    | PED019        |              2 |
    | PED020        |              6 |
    | PED021        |              1 |
    | PED022        |              3 |
    | PED023        |              4 |
    | PED024        |              2 |
    | PED025        |              3 |
    | PED026        |              5 |
    | PED027        |              2 |
    | PED028        |              3 |
    | PED029        |              4 |
    | PED030        |              3 |
    | PED031        |              4 |
    | PED032        |              2 |
    | PED033        |              3 |
    +---------------+----------------+
    33 rows in set (0.00 sec)
    ```

    

14. Devuelve un listado de los 20 productos más vendidos y el número total de unidades que se han vendido de cada uno. El listado deberá estar ordenado por el número total de unidades vendidas.

    ```mysql
    SELECT dp.codigo_producto, p.nombre, SUM(dp.cantidad) AS total_unidades_vendidas
    FROM detalle_pedido dp
    JOIN producto p ON dp.codigo_producto = p.codigo_producto
    GROUP BY dp.codigo_producto, p.nombre
    ORDER BY total_unidades_vendidas DESC
    LIMIT 20;
    +-----------------+----------------------------+-------------------------+
    | codigo_producto | nombre                     | total_unidades_vendidas |
    +-----------------+----------------------------+-------------------------+
    | PRD025          | Semillas de Tomate         |                       7 |
    | PRD026          | Kit de Plantación          |                       7 |
    | PRD023          | Caléndula                  |                       7 |
    | PRD005          | Cesta de Plantas Variadas  |                       6 |
    | PRD020          | Azalea                     |                       6 |
    | PRD001          | Ramo de Rosas Rojas        |                       5 |
    | PRD009          | Amapolas                   |                       5 |
    | PRD017          | Limón                      |                       5 |
    | PRD027          | Fertilizante Orgánico      |                       5 |
    | PRD004          | Orquídea Phalaenopsis      |                       4 |
    | PRD029          | Kit de Jardinería Infantil |                       4 |
    | PRD012          | Regadera                   |                       4 |
    | PRD016          | Manzano                    |                       4 |
    | PRD002          | Caja de Rosas Blancas      |                       3 |
    | PRD008          | Helechos                   |                       3 |
    | PRD011          | Tijeras de podar           |                       3 |
    | PRD030          | Set de Macetas de Cerámica |                       3 |
    | PRD028          | Terrario de Suculentas     |                       3 |
    | PRD015          | Albahaca                   |                       3 |
    | PRD018          | Naranjo                    |                       3 |
    +-----------------+----------------------------+-------------------------+
    20 rows in set (0.00 sec)
    ```

    

15. La facturación que ha tenido la empresa en toda la historia, indicando la base imponible, el IVA y el total facturado. La base imponible se calcula sumando el coste del producto por el número de unidades vendidas de la tabla detalle_pedido. El IVA es el 21 % de la base imponible, y el total la suma de los dos campos anteriores.

    ```mysql
    SELECT 
        SUM(dp.precio_unidad * dp.cantidad) AS base_imponible,
        SUM(dp.precio_unidad * dp.cantidad) * 0.21 AS iva,
        SUM(dp.precio_unidad * dp.cantidad) * 1.21 AS total_facturado
    FROM detalle_pedido dp;
    +----------------+----------+-----------------+
    | base_imponible | iva      | total_facturado |
    +----------------+----------+-----------------+
    |        2031.51 | 426.6171 |       2458.1271 |
    +----------------+----------+-----------------+
    1 row in set (0.00 sec)
    
    ```

    

16. La misma información que en la pregunta anterior, pero agrupada por código de producto.

    ```mysql
    SELECT 
        dp.codigo_producto,
        SUM(dp.precio_unidad * dp.cantidad) AS base_imponible,
        SUM(dp.precio_unidad * dp.cantidad) * 0.21 AS iva,
        SUM(dp.precio_unidad * dp.cantidad) * 1.21 AS total_facturado
    FROM detalle_pedido dp
    GROUP BY dp.codigo_producto;
    +-----------------+----------------+---------+-----------------+
    | codigo_producto | base_imponible | iva     | total_facturado |
    +-----------------+----------------+---------+-----------------+
    | PRD001          |          62.50 | 13.1250 |         75.6250 |
    | PRD002          |          47.97 | 10.0737 |         58.0437 |
    | PRD003          |          17.50 |  3.6750 |         21.1750 |
    | PRD004          |          43.96 |  9.2316 |         53.1916 |
    | PRD005          |          47.94 | 10.0674 |         58.0074 |
    | PRD006          |          18.50 |  3.8850 |         22.3850 |
    | PRD007          |          28.50 |  5.9850 |         34.4850 |
    | PRD008          |          62.97 | 13.2237 |         76.1937 |
    | PRD009          |          83.75 | 17.5875 |        101.3375 |
    | PRD010          |          23.98 |  5.0358 |         29.0158 |
    | PRD011          |          28.50 |  5.9850 |         34.4850 |
    | PRD012          |          91.00 | 19.1100 |        110.1100 |
    | PRD013          |          57.98 | 12.1758 |         70.1558 |
    | PRD014          |          32.50 |  6.8250 |         39.3250 |
    | PRD015          |          89.97 | 18.8937 |        108.8637 |
    | PRD016          |          71.96 | 15.1116 |         87.0716 |
    | PRD017          |          63.75 | 13.3875 |         77.1375 |
    | PRD018          |          64.50 | 13.5450 |         78.0450 |
    | PRD019          |          39.98 |  8.3958 |         48.3758 |
    | PRD020          |          87.00 | 18.2700 |        105.2700 |
    | PRD021          |          27.75 |  5.8275 |         33.5775 |
    | PRD023          |         193.97 | 40.7337 |        234.7037 |
    | PRD024          |          51.98 | 10.9158 |         62.8958 |
    | PRD025          |         180.21 | 37.8441 |        218.0541 |
    | PRD026          |         148.45 | 31.1745 |        179.6245 |
    | PRD027          |         118.97 | 24.9837 |        143.9537 |
    | PRD028          |          44.97 |  9.4437 |         54.4137 |
    | PRD029          |         127.00 | 26.6700 |        153.6700 |
    | PRD030          |          73.50 | 15.4350 |         88.9350 |
    +-----------------+----------------+---------+-----------------+
    29 rows in set (0.00 sec)
    ```

    

17. La misma información que en la pregunta anterior, pero agrupada por código de producto filtrada por los códigos que empiecen por OR.

    ```mysql
    SELECT 
        dp.codigo_producto,
        SUM(dp.precio_unidad * dp.cantidad) AS base_imponible,
        SUM(dp.precio_unidad * dp.cantidad) * 0.21 AS iva,
        SUM(dp.precio_unidad * dp.cantidad) * 1.21 AS total_facturado
    FROM detalle_pedido dp
    JOIN producto p ON dp.codigo_producto = p.codigo_producto
    WHERE p.codigo_producto LIKE 'OR%'
    GROUP BY dp.codigo_producto;
    Empty set (0,00 sec)
    
    ```

    

18. Lista las ventas totales de los productos que hayan facturado más de 3000 euros. Se mostrará el nombre, unidades vendidas, total facturado y total facturado con impuestos (21% IVA).

    ```mysql
    SELECT 
        p.nombre AS nombre_producto,
        SUM(dp.cantidad) AS unidades_vendidas,
        SUM(dp.precio_unidad * dp.cantidad) AS total_facturado,
        SUM(dp.precio_unidad * dp.cantidad) * 1.21 AS total_facturado_con_IVA
    FROM detalle_pedido dp
    JOIN producto p ON dp.codigo_producto = p.codigo_producto
    GROUP BY p.nombre
    HAVING total_facturado > 3000;
    Empty set (0,00 sec)
    ```

    

19. Muestre la suma total de todos los pagos que se realizaron para cada uno de los años que aparecen en la tabla pagos.

    ```mysql
    
    ```

    

### Subconsultas

##### Con operadores básicos de comparación

1. Devuelve el nombre del cliente con mayor límite de crédito.

   ```mysql
   SELECT nombre_cliente 
   FROM cliente 
   WHERE limite_credito = (
       SELECT MAX(limite_credito) FROM cliente
   );
   +----------------+
   | nombre_cliente |
   +----------------+
   | Tendo Garden   |
   +----------------+
   1 row in set (0.00 sec)
   ```
   
2. Devuelve el nombre del producto que tenga el precio de venta más caro.

   ```mysql
   SELECT nombre 
   FROM producto 
   WHERE precio_venta = (
       SELECT MAX(precio_venta) FROM producto
   );
   +-----------------------+
   | nombre                |
   +-----------------------+
   | Orquídea Phalaenopsis |
   +-----------------------+
   1 row in set (0.00 sec)
   ```
   
3. Devuelve el nombre del producto del que se han vendido más unidades. (Tenga en cuenta que tendrá que calcular cuál es el número total de unidades que se han vendido de cada producto a partir de los datos de la tabla detalle_pedido)

   ```mysql
   
   ```

     

4. Los clientes cuyo límite de crédito sea mayor que los pagos que haya realizado. (Sin utilizar INNER JOIN).

   ```mysql
   SELECT nombre_cliente
   FROM cliente
   WHERE limite_credito > (
       SELECT IFNULL(SUM(total_pago), 0)
       FROM pago
       WHERE pago.codigo_cliente = cliente.codigo_cliente
   );
   +--------------------------------+
   | nombre_cliente                 |
   +--------------------------------+
   | Tendo Garden                   |
   | Lasas S.A.                     |
   | Beragua                        |
   | Club Golf Puerta del hierro    |
   | Naturagua                      |
   | DaraDistribuciones             |
   | Madrileña de riegos            |
   | Lasas S.A.                     |
   | Camunas Jardines S.L.          |
   | Dardena S.A.                   |
   | Jardin de Flores               |
   | Flowers, S.A                   |
   | Naturajardin                   |
   | Golf S.A.                      |
   | Americh Golf Management SL     |
   | Aloha                          |
   | El Prat                        |
   | Sotogrande                     |
   | Vivero Humanes                 |
   | Fuenla City                    |
   | Jardines y Mansiones Cactus SL |
   | Jardinerías Matías SL          |
   | Top Campo                      |
   | Campohermoso                   |
   | france telecom                 |
   | Musée du Louvre                |
   | Tutifruti S.A                  |
   | Flores S.L.                    |
   | The Magic Garden               |
   | El Jardin Viviente S.L         |
   +--------------------------------+
   30 rows in set (0.00 sec)
   ```

   

5. Devuelve el producto que más unidades tiene en stock.

   ```mysql
   SELECT nombre
   FROM producto
   WHERE cantidad_stock = (
       SELECT MAX(cantidad_stock)
       FROM producto
   );
   +-------------------+
   | nombre            |
   +-------------------+
   | Ramo de Girasoles |
   | Kit de Plantación |
   +-------------------+
   2 rows in set (0.00 sec)
   ```

   

6. Devuelve el producto que menos unidades tiene en stock.

   ```mysql
   SELECT nombre
   FROM producto
   WHERE cantidad_stock = (
       SELECT MIN(cantidad_stock)
       FROM producto
   );
   +----------------------------+
   | nombre                     |
   +----------------------------+
   | Pala de jardín             |
   | Kit de Jardinería Infantil |
   +----------------------------+
   2 rows in set (0.00 sec)
   ```

   

7. Devuelve el nombre, los apellidos y el email de los empleados que están a cargo de Alberto Soria.

   ```mysql
   SELECT nombre_empleado, apellido1_empleado, apellido2_empleado, email_empleado
   FROM empleado
   WHERE codigo_jefe = (
       SELECT codigo_empleado
       FROM empleado
       WHERE nombre_empleado = 'Alberto' AND apellido1_empleado = 'Soria'
   );
   +-----------------+--------------------+--------------------+---------------------------+
   | nombre_empleado | apellido1_empleado | apellido2_empleado | email_empleado            |
   +-----------------+--------------------+--------------------+---------------------------+
   | Felipe          | Rosas              | Marquez            | frosas@jardineria.es      |
   | Juan Carlos     | Ortiz              | Serrano            | cortiz@jardineria.es      |
   | Carlos          | Soria              | Jimenez            | csoria@jardineria.es      |
   | Emmanuel        | Magaña             | Perez              | manu@jardineria.es        |
   | Francois        | Fignon             |                    | ffignon@gardening.com     |
   | Michael         | Bolton             |                    | mbolton@gardening.com     |
   | Hilary          | Washington         |                    | hwashington@gardening.com |
   | Nei             | Nishikori          |                    | nnishikori@gardening.com  |
   | Amy             | Johnson            |                    | ajohnson@gardening.com    |
   | Kevin           | Fallmer            |                    | kfalmer@gardening.com     |
   +-----------------+--------------------+--------------------+---------------------------+
   10 rows in set (0.00 sec)
   ```

     

  ##### Subconsultas con ALL y ANY
8. Devuelve el nombre del cliente con mayor límite de crédito.

   ```mysql
   SELECT nombre_cliente
   FROM cliente
   WHERE limite_credito >= ALL (
       SELECT limite_credito
       FROM cliente
   );
   +----------------+
   | nombre_cliente |
   +----------------+
   | Tendo Garden   |
   +----------------+
   1 row in set (0.00 sec)
   ```
   
   
   
9. Devuelve el nombre del producto que tenga el precio de venta más caro.

   ```mysql
   SELECT nombre
   FROM producto
   WHERE precio_venta >= ANY (
       SELECT precio_venta
       FROM producto
   );
   +----------------------------+
   | nombre                     |
   +----------------------------+
   | Ramo de Rosas Rojas        |
   | Caja de Rosas Blancas      |
   | Bouquet de Tulipanes       |
   | Orquídea Phalaenopsis      |
   | Cesta de Plantas Variadas  |
   | Ramo de Girasoles          |
   | Lavanda                    |
   | Helechos                   |
   | Amapolas                   |
   | Pala de jardín             |
   | Tijeras de podar           |
   | Regadera                   |
   | Menta                      |
   | Romero                     |
   | Albahaca                   |
   | Manzano                    |
   | Limón                      |
   | Naranjo                    |
   | Dalia                      |
   | Azalea                     |
   | Begonia                    |
   | Caléndula                  |
   | Petunia                    |
   | Semillas de Tomate         |
   | Kit de Plantación          |
   | Fertilizante Orgánico      |
   | Terrario de Suculentas     |
   | Kit de Jardinería Infantil |
   | Set de Macetas de Cerámica |
   +----------------------------+
   29 rows in set (0.00 sec)
   ```
   
10. Devuelve el producto que menos unidades tiene en stock.

    ```mysql
    SELECT nombre
    FROM producto
    WHERE cantidad_stock <= ALL (
        SELECT cantidad_stock
        FROM producto
    );
    +----------------------------+
    | nombre                     |
    +----------------------------+
    | Pala de jardín             |
    | Kit de Jardinería Infantil |
    +----------------------------+
    2 rows in set (0.00 sec)
    ```
    
    
    
    ##### Subconsultas con IN y NOT IN
11. Devuelve el nombre, apellido1 y cargo de los empleados que no representen a ningún cliente.

    ```mysql
    SELECT nombre_empleado, apellido1_empleado, apellido2_empleado, puesto_empleado.nombre_puesto
    FROM empleado
    JOIN puesto_empleado ON empleado.codigo_puesto_empleado = puesto_empleado.codigo_puesto_empleado
    WHERE codigo_empleado NOT IN (
        SELECT DISTINCT codigo_rep_ventas
        FROM cliente
        WHERE codigo_rep_ventas IS NOT NULL
    );
    +-----------------+--------------------+--------------------+-----------------------+
    | nombre_empleado | apellido1_empleado | apellido2_empleado | nombre_puesto         |
    +-----------------+--------------------+--------------------+-----------------------+
    | Marcos          | Magaña             | Perez              | Director General      |
    | Ruben           | López              | Martinez           | Subdirector Marketing |
    | Alberto         | Soria              | Carrasco           | Subdirector Ventas    |
    | Maria           | Solís              | Jerez              | Secretaria            |
    | Carlos          | Soria              | Jimenez            | Director Oficina      |
    | Emmanuel        | Magaña             | Perez              | Director Oficina      |
    | Francois        | Fignon             |                    | Director Oficina      |
    | Michael         | Bolton             |                    | Director Oficina      |
    | Hilary          | Washington         |                    | Director Oficina      |
    | Nei             | Nishikori          |                    | Director Oficina      |
    | Amy             | Johnson            |                    | Director Oficina      |
    | Kevin           | Fallmer            |                    | Director Oficina      |
    +-----------------+--------------------+--------------------+-----------------------+
    12 rows in set (0.00 sec)
    ```
    
12. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.

    ```mysql
    SELECT codigo_cliente, nombre_cliente, codigo_ciudad, codigo_postal, limite_credito, codigo_rep_ventas
    FROM cliente
    WHERE codigo_cliente NOT IN (
        SELECT DISTINCT codigo_cliente
        FROM pago
    );
    +----------------+-----------------------------+---------------+---------------+----------------+-------------------+
    | codigo_cliente | nombre_cliente              | codigo_ciudad | codigo_postal | limite_credito | codigo_rep_ventas |
    +----------------+-----------------------------+---------------+---------------+----------------+-------------------+
    |              6 | Lasas S.A.                  |             2 | 28945         |      154310.00 |                12 |
    |              8 | Club Golf Puerta del hierro |             3 | 28930         |       40000.00 |                14 |
    |             10 | DaraDistribuciones          |             9 | 28946         |       50000.00 |                16 |
    |             11 | Madrileña de riegos         |             8 | 28943         |       20000.00 |                17 |
    |             12 | Lasas S.A.                  |             5 | 28945         |       15431.00 |                19 |
    |             17 | Flowers, S.A                |             2 | 24586         |        3500.00 |                25 |
    |             18 | Naturajardin                |             1 | 28011         |        5050.00 |                27 |
    |             20 | Americh Golf Management SL  |             8 | 12320         |       20000.00 |                30 |
    |             21 | Aloha                       |             6 | 35488         |       50000.00 |                31 |
    |             22 | El Prat                     |             5 | 12320         |       30000.00 |                10 |
    |             24 | Vivero Humanes              |             1 | 28970         |        7430.00 |                24 |
    |             25 | Fuenla City                 |             7 | 28574         |        4500.00 |                24 |
    |             29 | Top Campo                   |             2 | 28574         |        5500.00 |                31 |
    |             31 | Campohermoso                |             8 | 28945         |        3250.00 |                 5 |
    |             32 | france telecom              |             6 | 75010         |       10000.00 |                 6 |
    |             33 | Musée du Louvre             |             2 | 75058         |       30000.00 |                 9 |
    |             36 | Flores S.L.                 |             9 | 29643         |        6000.00 |                12 |
    |             37 | The Magic Garden            |             8 | 65930         |       10000.00 |                 8 |
    +----------------+-----------------------------+---------------+---------------+----------------+-------------------+
    18 rows in set (0.00 sec)
    ```
    
13. Devuelve un listado que muestre solamente los clientes que sí han realizado algún pago.

    ```mysql
    SELECT codigo_cliente, nombre_cliente, codigo_ciudad, codigo_postal, limite_credito, codigo_rep_ventas
    FROM cliente
    WHERE codigo_cliente IN (
        SELECT DISTINCT codigo_cliente
        FROM pago
    );
    +----------------+--------------------------------+---------------+---------------+----------------+-------------------+
    | codigo_cliente | nombre_cliente                 | codigo_ciudad | codigo_postal | limite_credito | codigo_rep_ventas |
    +----------------+--------------------------------+---------------+---------------+----------------+-------------------+
    |              1 | GoldFish Garden                |             1 | 24006         |        3000.00 |                 5 |
    |              3 | Gardening Associates           |             1 | 24010         |        6000.00 |                 6 |
    |              4 | Gerudo Valley                  |             1 | 85495         |       12000.00 |                 9 |
    |              5 | Tendo Garden                   |             2 | 696969        |      600000.00 |                 8 |
    |              7 | Beragua                        |             3 | 28942         |       20000.00 |                13 |
    |              9 | Naturagua                      |             4 | 28947         |       32000.00 |                16 |
    |             13 | Camunas Jardines S.L.          |             9 | 28145         |       16481.00 |                21 |
    |             14 | Dardena S.A.                   |             6 | 28003         |      321000.00 |                22 |
    |             15 | Jardin de Flores               |             5 | 28950         |       40000.00 |                24 |
    |             16 | Flores Marivi                  |             3 | 28945         |        1500.00 |                24 |
    |             19 | Golf S.A.                      |             9 | 38297         |       30000.00 |                28 |
    |             23 | Sotogrande                     |             2 | 11310         |       60000.00 |                16 |
    |             26 | Jardines y Mansiones Cactus SL |             6 | 29874         |       76000.00 |                25 |
    |             27 | Jardinerías Matías SL          |             5 | 37845         |      100500.00 |                24 |
    |             28 | Agrojardin                     |             3 | 28904         |        8040.00 |                30 |
    |             30 | Jardineria Sara                |             4 | 27584         |        7500.00 |                10 |
    |             35 | Tutifruti S.A                  |             1 | 2000          |       10000.00 |                24 |
    |             38 | El Jardin Viviente S.L         |             5 | 2003          |        8000.00 |                24 |
    +----------------+--------------------------------+---------------+---------------+----------------+-------------------+
    18 rows in set (0.00 sec)
    ```
    
14. Devuelve un listado de los productos que nunca han aparecido en un pedido.

    ```mysql
    SELECT codigo_producto, nombre, codigo_gama, cantidad_stock, precio_venta, descripcion, codigo_dimensiones
    FROM producto
    WHERE codigo_producto NOT IN (
        SELECT DISTINCT codigo_producto
        FROM detalle_pedido
    );
    Empty set (0.00 sec)
    ```
    
15. Devuelve el nombre, apellidos, puesto y teléfono de la oficina de aquellos empleados que no sean representante de ventas de ningún cliente.

    ```mysql
    SELECT e.nombre_empleado, e.apellido1_empleado, e.apellido2_empleado, p.nombre_puesto, t.telefono_oficina
    FROM empleado e
    JOIN puesto_empleado p ON e.codigo_puesto_empleado = p.codigo_puesto_empleado
    JOIN oficina o ON e.codigo_oficina = o.codigo_oficina
    JOIN telefono_oficina t ON o.codigo_oficina = t.codigo_oficina
    WHERE e.codigo_empleado NOT IN (
        SELECT DISTINCT codigo_rep_ventas
        FROM cliente
    );
    +-----------------+--------------------+--------------------+-----------------------+------------------+
    | nombre_empleado | apellido1_empleado | apellido2_empleado | nombre_puesto         | telefono_oficina |
    +-----------------+--------------------+--------------------+-----------------------+------------------+
    | Emmanuel        | Magaña             | Perez              | Director Oficina      | +34 93 3561182   |
    | Hilary          | Washington         |                    | Director Oficina      | +1 215 837 0825  |
    | Amy             | Johnson            |                    | Director Oficina      | +44 20 78772041  |
    | Carlos          | Soria              | Jimenez            | Director Oficina      | +34 91 7514487   |
    | Francois        | Fignon             |                    | Director Oficina      | +33 14 723 4404  |
    | Michael         | Bolton             |                    | Director Oficina      | +1 650 219 4782  |
    | Kevin           | Fallmer            |                    | Director Oficina      | +61 2 9264 2451  |
    | Marcos          | Magaña             | Perez              | Director General      | +34 925 867231   |
    | Ruben           | López              | Martinez           | Subdirector Marketing | +34 925 867231   |
    | Alberto         | Soria              | Carrasco           | Subdirector Ventas    | +34 925 867231   |
    | Maria           | Solís              | Jerez              | Secretaria            | +34 925 867231   |
    | Nei             | Nishikori          |                    | Director Oficina      | +81 33 224 5000  |
    +-----------------+--------------------+--------------------+-----------------------+------------------+
    12 rows in set (0.00 sec)
    ```
    
16. Devuelve las oficinas donde no trabajan ninguno de los empleados que hayan sido los representantes de ventas de algún cliente que haya realizado la compra de algún producto de la gama Frutales.

    ```mysql
    
    ```

17. Devuelve un listado con los clientes que han realizado algún pedido pero no han realizado ningún pago.
    
    ```mysql
    SELECT DISTINCT codigo_cliente, nombre_cliente
    FROM cliente
    WHERE codigo_cliente IN (
        SELECT DISTINCT codigo_cliente
        FROM pedido
    ) 
    AND codigo_cliente NOT IN (
        SELECT DISTINCT codigo_cliente
        FROM pago
    );
    +----------------+-----------------------------+
    | codigo_cliente | nombre_cliente              |
    +----------------+-----------------------------+
    |              6 | Lasas S.A.                  |
    |              8 | Club Golf Puerta del hierro |
    |             10 | DaraDistribuciones          |
    |             11 | Madrileña de riegos         |
    |             12 | Lasas S.A.                  |
    |             17 | Flowers, S.A                |
    |             18 | Naturajardin                |
    |             20 | Americh Golf Management SL  |
    |             21 | Aloha                       |
    |             22 | El Prat                     |
    |             24 | Vivero Humanes              |
    |             25 | Fuenla City                 |
    |             29 | Top Campo                   |
    |             31 | Campohermoso                |
    |             32 | france telecom              |
    |             33 | Musée du Louvre             |
    |             36 | Flores S.L.                 |
    |             37 | The Magic Garden            |
    +----------------+-----------------------------+
    18 rows in set (0.00 sec)
    ```
    
    

##### Subconsultas con EXISTS y NOT EXISTS

18. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.

    ```mysql
    SELECT codigo_cliente, nombre_cliente
    FROM cliente c
    WHERE NOT EXISTS (
        SELECT codigo_pago 
        FROM pago p
        WHERE p.codigo_cliente = c.codigo_cliente
    );
    +----------------+-----------------------------+
    | codigo_cliente | nombre_cliente              |
    +----------------+-----------------------------+
    |              6 | Lasas S.A.                  |
    |              8 | Club Golf Puerta del hierro |
    |             10 | DaraDistribuciones          |
    |             11 | Madrileña de riegos         |
    |             12 | Lasas S.A.                  |
    |             17 | Flowers, S.A                |
    |             18 | Naturajardin                |
    |             20 | Americh Golf Management SL  |
    |             21 | Aloha                       |
    |             22 | El Prat                     |
    |             24 | Vivero Humanes              |
    |             25 | Fuenla City                 |
    |             29 | Top Campo                   |
    |             31 | Campohermoso                |
    |             32 | france telecom              |
    |             33 | Musée du Louvre             |
    |             36 | Flores S.L.                 |
    |             37 | The Magic Garden            |
    +----------------+-----------------------------+
    18 rows in set (0.00 sec)
    ```
    
19. Devuelve un listado que muestre solamente los clientes que sí han realizado algún pago.

    ```mysql
    SELECT codigo_cliente, nombre_cliente
    FROM cliente c
    WHERE EXISTS (
        SELECT codigo_pago
        FROM pago p
        WHERE p.codigo_cliente = c.codigo_cliente
    );
    +----------------+--------------------------------+
    | codigo_cliente | nombre_cliente                 |
    +----------------+--------------------------------+
    |              1 | GoldFish Garden                |
    |              3 | Gardening Associates           |
    |              4 | Gerudo Valley                  |
    |              5 | Tendo Garden                   |
    |              7 | Beragua                        |
    |              9 | Naturagua                      |
    |             13 | Camunas Jardines S.L.          |
    |             14 | Dardena S.A.                   |
    |             15 | Jardin de Flores               |
    |             16 | Flores Marivi                  |
    |             19 | Golf S.A.                      |
    |             23 | Sotogrande                     |
    |             26 | Jardines y Mansiones Cactus SL |
    |             27 | Jardinerías Matías SL          |
    |             28 | Agrojardin                     |
    |             30 | Jardineria Sara                |
    |             35 | Tutifruti S.A                  |
    |             38 | El Jardin Viviente S.L         |
    +----------------+--------------------------------+
    18 rows in set (0.00 sec)
    ```
    
20. Devuelve un listado de los productos que nunca han aparecido en un pedido.

    ```mysql
    SELECT codigo_producto, nombre
    FROM producto p
    WHERE NOT EXISTS (
        SELECT dp.codigo_pedido
        FROM detalle_pedido dp
        WHERE dp.codigo_producto = p.codigo_producto
    );
    Empty set (0.00 sec)
    ```
    
21. Devuelve un listado de los productos que han aparecido en un pedido alguna vez.

    ```mysql
    SELECT DISTINCT codigo_producto, nombre
    FROM producto p
    WHERE EXISTS (
        SELECT dp.codigo_pedido
        FROM detalle_pedido dp
        WHERE dp.codigo_producto = p.codigo_producto
    );
    +-----------------+----------------------------+
    | codigo_producto | nombre                     |
    +-----------------+----------------------------+
    | PRD001          | Ramo de Rosas Rojas        |
    | PRD002          | Caja de Rosas Blancas      |
    | PRD003          | Bouquet de Tulipanes       |
    | PRD004          | Orquídea Phalaenopsis      |
    | PRD005          | Cesta de Plantas Variadas  |
    | PRD006          | Ramo de Girasoles          |
    | PRD007          | Lavanda                    |
    | PRD008          | Helechos                   |
    | PRD009          | Amapolas                   |
    | PRD010          | Pala de jardín             |
    | PRD011          | Tijeras de podar           |
    | PRD012          | Regadera                   |
    | PRD013          | Menta                      |
    | PRD014          | Romero                     |
    | PRD015          | Albahaca                   |
    | PRD016          | Manzano                    |
    | PRD017          | Limón                      |
    | PRD018          | Naranjo                    |
    | PRD019          | Dalia                      |
    | PRD020          | Azalea                     |
    | PRD021          | Begonia                    |
    | PRD023          | Caléndula                  |
    | PRD024          | Petunia                    |
    | PRD025          | Semillas de Tomate         |
    | PRD026          | Kit de Plantación          |
    | PRD027          | Fertilizante Orgánico      |
    | PRD028          | Terrario de Suculentas     |
    | PRD029          | Kit de Jardinería Infantil |
    | PRD030          | Set de Macetas de Cerámica |
    +-----------------+----------------------------+
    29 rows in set (0.00 sec)
    ```
    
    

### Consultas variadas 

1. Devuelve el listado de clientes indicando el nombre del cliente y cuántos pedidos ha realizado. Tenga en cuenta que pueden existir clientes que no han realizado ningún pedido.

   ```mysql
   SELECT c.nombre_cliente,COUNT(p.codigo_pedido) AS total_pedidos
   FROM cliente c
   LEFT JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
   GROUP BY c.codigo_cliente;
   +--------------------------------+---------------+
   | nombre_cliente                 | total_pedidos |
   +--------------------------------+---------------+
   | GoldFish Garden                |             2 |
   | Gardening Associates           |             1 |
   | Gerudo Valley                  |             2 |
   | Tendo Garden                   |             1 |
   | Lasas S.A.                     |             1 |
   | Beragua                        |             2 |
   | Club Golf Puerta del hierro    |             1 |
   | Naturagua                      |             1 |
   | DaraDistribuciones             |             1 |
   | Madrileña de riegos            |             1 |
   | Lasas S.A.                     |             1 |
   | Camunas Jardines S.L.          |             1 |
   | Dardena S.A.                   |             1 |
   | Jardin de Flores               |             1 |
   | Flores Marivi                  |             1 |
   | Flowers, S.A                   |             1 |
   | Naturajardin                   |             1 |
   | Golf S.A.                      |             1 |
   | Americh Golf Management SL     |             1 |
   | Aloha                          |             1 |
   | El Prat                        |             1 |
   | Sotogrande                     |             1 |
   | Vivero Humanes                 |             1 |
   | Fuenla City                    |             1 |
   | Jardines y Mansiones Cactus SL |             1 |
   | Jardinerías Matías SL          |             1 |
   | Agrojardin                     |             1 |
   | Top Campo                      |             1 |
   | Jardineria Sara                |             1 |
   | Campohermoso                   |             1 |
   | france telecom                 |             1 |
   | Musée du Louvre                |             1 |
   | Tutifruti S.A                  |             2 |
   | Flores S.L.                    |             1 |
   | The Magic Garden               |             1 |
   | El Jardin Viviente S.L         |             1 |
   +--------------------------------+---------------+
   36 rows in set (0.00 sec)
   ```

   

2. Devuelve un listado con los nombres de los clientes y el total pagado por cada uno de ellos. Tenga en cuenta que pueden existir clientes que no han realizado ningún pago.

   ```mysql
   SELECT c.nombre_cliente, COALESCE(SUM(p.total_pago), 0) AS total_pagado
   FROM cliente c
   LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente
   GROUP BY c.codigo_cliente;
   +--------------------------------+--------------+
   | nombre_cliente                 | total_pagado |
   +--------------------------------+--------------+
   | GoldFish Garden                |      4000.00 |
   | Gardening Associates           |     10926.00 |
   | Gerudo Valley                  |     81849.00 |
   | Tendo Garden                   |     23794.00 |
   | Lasas S.A.                     |         0.00 |
   | Beragua                        |      2390.00 |
   | Club Golf Puerta del hierro    |         0.00 |
   | Naturagua                      |       929.00 |
   | DaraDistribuciones             |         0.00 |
   | Madrileña de riegos            |         0.00 |
   | Lasas S.A.                     |         0.00 |
   | Camunas Jardines S.L.          |      2246.00 |
   | Dardena S.A.                   |      4160.00 |
   | Jardin de Flores               |     12081.00 |
   | Flores Marivi                  |      4399.00 |
   | Flowers, S.A                   |         0.00 |
   | Naturajardin                   |         0.00 |
   | Golf S.A.                      |       232.00 |
   | Americh Golf Management SL     |         0.00 |
   | Aloha                          |         0.00 |
   | El Prat                        |         0.00 |
   | Sotogrande                     |       272.00 |
   | Vivero Humanes                 |         0.00 |
   | Fuenla City                    |         0.00 |
   | Jardines y Mansiones Cactus SL |     18846.00 |
   | Jardinerías Matías SL          |     10972.00 |
   | Agrojardin                     |      8489.00 |
   | Top Campo                      |         0.00 |
   | Jardineria Sara                |      7863.00 |
   | Campohermoso                   |         0.00 |
   | france telecom                 |         0.00 |
   | Musée du Louvre                |         0.00 |
   | Tutifruti S.A                  |      3321.00 |
   | Flores S.L.                    |         0.00 |
   | The Magic Garden               |         0.00 |
   | El Jardin Viviente S.L         |      1171.00 |
   +--------------------------------+--------------+
   36 rows in set (0.00 sec)
   ```

   

3. Devuelve el nombre de los clientes que hayan hecho pedidos en 2008 ordenados alfabéticamente de menor a mayor.

   ```mysql
   SELECT DISTINCT c.nombre_cliente
   FROM cliente c
   INNER JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
   WHERE YEAR(p.fecha_pedido) = 2008
   ORDER BY c.nombre_cliente ASC;
   Empty set (0,00 sec)
   ```

   

4. Devuelve el nombre del cliente, el nombre y primer apellido de su representante de ventas y el número de teléfono de la oficina del representante de ventas, de aquellos clientes que no hayan realizado ningún pago.

   ```mysql
   SELECT c.nombre_cliente, e.nombre_empleado, e.apellido1_empleado, t.telefono_oficina
   FROM cliente c
   LEFT JOIN empleado e ON c.codigo_rep_ventas = e.codigo_empleado
   LEFT JOIN telefono_oficina t ON e.codigo_oficina = t.codigo_oficina
   LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente
   WHERE p.codigo_pago IS NULL;
   +-----------------------------+-----------------+--------------------+------------------+
   | nombre_cliente              | nombre_empleado | apellido1_empleado | telefono_oficina |
   +-----------------------------+-----------------+--------------------+------------------+
   | Lasas S.A.                  | José Manuel     | Martinez           | +34 93 3561182   |
   | Club Golf Puerta del hierro | Oscar           | Palma              | +34 93 3561182   |
   | DaraDistribuciones          | Lionel          | Narvaez            | +33 14 723 4404  |
   | Madrileña de riegos         | Laurent         | Serra              | +33 14 723 4404  |
   | Lasas S.A.                  | Walter Santiago | Sanchez            | +1 650 219 4782  |
   | Flowers, S.A                | Takuma          | Nomura             | +81 33 224 5000  |
   | Naturajardin                | Larry           | Westfalls          | +44 20 78772041  |
   | Americh Golf Management SL  | Julian          | Bellinelli         | +61 2 9264 2451  |
   | Aloha                       | Mariko          | Kishi              | +61 2 9264 2451  |
   | El Prat                     | Hilario         | Rodriguez          | +34 91 7514487   |
   | Vivero Humanes              | Narumi          | Riko               | +81 33 224 5000  |
   | Fuenla City                 | Narumi          | Riko               | +81 33 224 5000  |
   | Top Campo                   | Mariko          | Kishi              | +61 2 9264 2451  |
   | Campohermoso                | Felipe          | Rosas              | +34 925 867231   |
   | france telecom              | Juan Carlos     | Ortiz              | +34 925 867231   |
   | Musée du Louvre             | Lucio           | Campoamor          | +34 91 7514487   |
   | Flores S.L.                 | José Manuel     | Martinez           | +34 93 3561182   |
   | The Magic Garden            | Mariano         | López              | +34 91 7514487   |
   +-----------------------------+-----------------+--------------------+------------------+
   18 rows in set (0.00 sec)
   ```

   

5. Devuelve el listado de clientes donde aparezca el nombre del cliente, el nombre y primer apellido de su representante de ventas y la ciudad donde está su oficina.

   ```mysql
   SELECT c.nombre_cliente, CONCAT(e.nombre_empleado,' ', e.apellido1_empleado) AS representante_ventas, ci.nombre_ciudad AS ciudad_oficina
   FROM cliente c
   JOIN empleado e ON c.codigo_rep_ventas = e.codigo_empleado
   JOIN oficina o ON e.codigo_oficina = o.codigo_oficina
   JOIN ciudad ci ON o.codigo_ciudad = ci.codigo_ciudad;
   +--------------------------------+-------------------------+----------------------+
   | nombre_cliente                 | representante_ventas    | ciudad_oficina       |
   +--------------------------------+-------------------------+----------------------+
   | Lasas S.A.                     | José Manuel Martinez    | Barcelona            |
   | Flores S.L.                    | José Manuel Martinez    | Barcelona            |
   | Beragua                        | David Palma             | Barcelona            |
   | Club Golf Puerta del hierro    | Oscar Palma             | Barcelona            |
   | Camunas Jardines S.L.          | Marcus Paxton           | Boston
         |
   | Dardena S.A.                   | Lorena Paxton           | Boston
         |
   | Naturajardin                   | Larry Westfalls         | Londres              |
   | Golf S.A.                      | John Walton             | Londres              |
   | Tendo Garden                   | Mariano López           | Madrid
         |
   | The Magic Garden               | Mariano López           | Madrid
         |
   | Gerudo Valley                  | Lucio Campoamor         | Madrid
         |
   | Musée du Louvre                | Lucio Campoamor         | Madrid
         |
   | El Prat                        | Hilario Rodriguez       | Madrid
         |
   | Jardineria Sara                | Hilario Rodriguez       | Madrid
         |
   | Naturagua                      | Lionel Narvaez          | Paris
         |
   | DaraDistribuciones             | Lionel Narvaez          | Paris
         |
   | Sotogrande                     | Lionel Narvaez          | Paris
         |
   | Madrileña de riegos            | Laurent Serra           | Paris
         |
   | Lasas S.A.                     | Walter Santiago Sanchez | San Francisco        |
   | Americh Golf Management SL     | Julian Bellinelli       | Sydney
         |
   | Agrojardin                     | Julian Bellinelli       | Sydney
         |
   | Aloha                          | Mariko Kishi            | Sydney
         |
   | Top Campo                      | Mariko Kishi            | Sydney
         |
   | GoldFish Garden                | Felipe Rosas            | Talavera de la Reina |
   | Campohermoso                   | Felipe Rosas            | Talavera de la Reina |
   | Gardening Associates           | Juan Carlos Ortiz       | Talavera de la Reina |
   | france telecom                 | Juan Carlos Ortiz       | Talavera de la Reina |
   | Jardin de Flores               | Narumi Riko             | Tokyo
         |
   | Flores Marivi                  | Narumi Riko             | Tokyo
         |
   | Vivero Humanes                 | Narumi Riko             | Tokyo
         |
   | Fuenla City                    | Narumi Riko             | Tokyo
         |
   | Jardinerías Matías SL          | Narumi Riko             | Tokyo
         |
   | Tutifruti S.A                  | Narumi Riko             | Tokyo
         |
   | El Jardin Viviente S.L         | Narumi Riko             | Tokyo
         |
   | Flowers, S.A                   | Takuma Nomura           | Tokyo
         |
   | Jardines y Mansiones Cactus SL | Takuma Nomura           | Tokyo
         |
   +--------------------------------+-------------------------+----------------------+
   36 rows in set (0.00 sec)
   ```

   

6. Devuelve el nombre, apellidos, puesto y teléfono de la oficina de aquellos empleados que no sean representante de ventas de ningún cliente.

   ```mysql
   SELECT CONCAT(e.nombre_empleado,' ', e.apellido1_empleado,' ', ifnull(e.apellido2_empleado,'')) AS no_rep_ventas, pu.nombre_puesto AS puesto, t.telefono_oficina AS telefono_oficina
   FROM empleado e
   JOIN puesto_empleado pu ON e.codigo_puesto_empleado = pu.codigo_puesto_empleado
   LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_rep_ventas
   LEFT JOIN oficina o ON e.codigo_oficina = o.codigo_oficina
   LEFT JOIN telefono_oficina t ON o.codigo_oficina = t.codigo_oficina
   WHERE c.codigo_cliente IS NULL;
   +------------------------+-----------------------+------------------+
   | no_rep_ventas          | puesto                | telefono_oficina |
   +------------------------+-----------------------+------------------+
   | Marcos Magaña Perez    | Director General      | +34 925 867231   |
   | Ruben López Martinez   | Subdirector Marketing | +34 925 867231   |
   | Alberto Soria Carrasco | Subdirector Ventas    | +34 925 867231   |
   | Maria Solís Jerez      | Secretaria            | +34 925 867231   |
   | Carlos Soria Jimenez   | Director Oficina      | +34 91 7514487   |
   | Emmanuel Magaña Perez  | Director Oficina      | +34 93 3561182   |
   | Francois Fignon        | Director Oficina      | +33 14 723 4404  |
   | Michael Bolton         | Director Oficina      | +1 650 219 4782  |
   | Hilary Washington      | Director Oficina      | +1 215 837 0825  |
   | Nei Nishikori          | Director Oficina      | +81 33 224 5000  |
   | Amy Johnson            | Director Oficina      | +44 20 78772041  |
   | Kevin Fallmer          | Director Oficina      | +61 2 9264 2451  |
   +------------------------+-----------------------+------------------+
   12 rows in set (0.00 sec)
   ```

   

7. Devuelve un listado indicando todas las ciudades donde hay oficinas y el número de empleados que tiene.

   ```mysql
   SELECT c.nombre_ciudad AS ciudad, COUNT(*) AS numero_empleados
   FROM ciudad c
   JOIN oficina o ON c.codigo_ciudad = o.codigo_ciudad
   JOIN empleado e ON o.codigo_oficina = e.codigo_oficina
   GROUP BY c.nombre_ciudad;
   +----------------------+------------------+
   | ciudad               | numero_empleados |
   +----------------------+------------------+
   | Barcelona            |                4 |
   | Boston               |                3 |
   | Londres              |                3 |
   | Madrid               |                4 |
   | Paris                |                3 |
   | San Francisco        |                2 |
   | Sydney               |                3 |
   | Talavera de la Reina |                6 |
   | Tokyo                |                3 |
   +----------------------+------------------+
   9 rows in set (0.00 sec)
   ```




### VISTAS

1. Vista para la consulta Devuelve un listado indicando todas las ciudades donde hay oficinas y el número de empleados que tiene.

   ```mysql
   -- Creación Vista
   CREATE VIEW vista_numero_empleados_por_ciudad AS
   SELECT c.nombre_ciudad AS ciudad, COUNT(*) AS numero_empleados
   FROM ciudad c
   JOIN oficina o ON c.codigo_ciudad = o.codigo_ciudad
   JOIN empleado e ON o.codigo_oficina = e.codigo_oficina
   GROUP BY c.nombre_ciudad;
   
   -- Llamado de la vista
   SELECT ciudad,numero_empleados FROM vista_numero_empleados_por_ciudad;
   
   ```

   

2.  Vista para la consulta Devuelve un listado con los nombres de los clientes y el total pagado por cada uno de ellos. Tenga en cuenta que pueden existir clientes que no han realizado ningún pago.

   ```mysql
   -- Creación Vista
   CREATE VIEW clientes_y_pagos AS
   SELECT c.nombre_cliente, COALESCE(SUM(p.total_pago), 0) AS total_pagado
   FROM cliente c
   LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente
   GROUP BY c.codigo_cliente;
   
   -- Llamado de la vista
   SELECT nombre_cliente, total_pagado FROM clientes_y_pagos;
   ```

   

3. Vista para la consulta Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.

   ```mysql
   -- Creación Vista
   CREATE VIEW clientes_sin_pagos AS
   SELECT codigo_cliente, nombre_cliente
   FROM cliente c
   WHERE NOT EXISTS (
       SELECT codigo_pago 
       FROM pago p
       WHERE p.codigo_cliente = c.codigo_cliente
   );
   
   -- Llamado de la vista
   SELECT codigo_cliente, nombre_cliente FROM clientes_sin_pagos;
   ```

   

4. Vista para la consulta Devuelve el producto que menos unidades tiene en stock.

   ```mysql
   -- Creación Vista
   CREATE VIEW productos_bajo_stock AS
   SELECT nombre
   FROM producto
   WHERE cantidad_stock = (
       SELECT MIN(cantidad_stock)
       FROM producto
   );
   
   -- Llamado de la vista
   SELECT nombre FROM productos_bajo_stock;
   ```

   

5. Vista para la consulta La facturación que ha tenido la empresa en toda la historia, indicando la base imponible, el IVA y el total facturado. La base imponible se calcula sumando el coste del producto por el número de unidades vendidas de la tabla detalle_pedido. El IVA es el 21 % de la base imponible, y el total la suma de los dos campos anteriores.

   ```mysql
   -- Creación Vista
   CREATE VIEW historico_facturacion_empresa AS
   SELECT 
       SUM(dp.precio_unidad * dp.cantidad) AS base_imponible,
       SUM(dp.precio_unidad * dp.cantidad) * 0.21 AS iva,
       SUM(dp.precio_unidad * dp.cantidad) * 1.21 AS total_facturado
   FROM detalle_pedido dp;
   
   -- Llamado de la vista
   SELECT base_imponible, iva, total_facturado FROM historico_facturacion_empresa;
   ```

   

6. Vista para la consulta Devuelve un listado con el código de oficina y la ciudad donde hay oficinas.

   ```mysql
   -- Creación Vista
   CREATE VIEW ciudades_oficina AS
   SELECT o.codigo_oficina, c.nombre_ciudad
   FROM oficina o
   JOIN ciudad c ON o.codigo_ciudad = c.codigo_ciudad;
   
   -- Llamado de la vista
   SELECT codigo_oficina, nombre_ciudad FROM ciudades_oficina;
   ```

   

7. Vista para la consulta Devuelve un listado con el nombre de los todos los clientes españoles.

   ```mysql
   -- Creación Vista
   CREATE VIEW clientes_espanoles AS
   SELECT cli.nombre_cliente
   FROM cliente cli
   JOIN ciudad c ON cli.codigo_ciudad = c.codigo_ciudad
   JOIN region r ON c.codigo_region = r.codigo_region
   JOIN pais p ON r.codigo_pais = p.codigo_pais
   WHERE p.nombre_pais = 'España';
   
   -- Llamado de la vista
   SELECT nombre_cliente FROM clientes_espanoles;
   ```

   

8. Vistas para la consulta Obtén un listado con el nombre de cada cliente y el nombre y apellido de su representante de ventas.

   ```mysql
   -- Creación Vista
   CREATE VIEW cliente_representante AS
   SELECT c.nombre_cliente, CONCAT(e.nombre_empleado,' ', e.apellido1_empleado,' ', ifnull(e.apellido2_empleado,'')) AS representante_ventas
   FROM cliente AS c
   INNER JOIN empleado AS e ON c.codigo_rep_ventas = e.codigo_empleado;
   
   -- Llamado de la vista
   SELECT nombre_cliente, representante_ventas FROM cliente_representante;
   ```

    

9. Devuelve el nombre del puesto, nombre, apellidos y email del jefe de la empresa.

   ```mysql
   -- Creación Vista
   CREATE VIEW jefe AS
   SELECT pe.nombre_puesto, CONCAT(e.nombre_empleado,' ', e.apellido1_empleado,' ', ifnull(e.apellido2_empleado,'')) AS nombre_empleado, e.email_empleado
   FROM empleado e
   JOIN puesto_empleado pe ON e.codigo_puesto_empleado = pe.codigo_puesto_empleado
   WHERE e.codigo_jefe IS NULL;
   
   
   -- Llamado de la vista
   SELECT nombre_puesto, nombre_empleado, email_empleado FROM jefe;
   ```

   

10. Devuelve un listado con el nombre de los empleados junto con el nombre de sus jefes.

    ```mysql
    -- Creación Vista
    CREATE VIEW empleado_jefe AS
    SELECT CONCAT(e.nombre_empleado,' ', e.apellido1_empleado,' ', ifnull(e.apellido2_empleado,'')) AS empleado, CONCAT(j.nombre_empleado,' ', j.apellido1_empleado,' ', ifnull(j.apellido2_empleado,'')) AS nombre_jefe
    FROM empleado e
    INNER JOIN empleado j ON e.codigo_jefe = j.codigo_empleado;
    
    -- Llamado de la vista
    SELECT empleado, nombre_jefe FROM empleado_jefe;
    ```

    

#### PROCEDIMIENTO ALMACENADOS

1. Insertar país

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS insert_pais;
   CREATE PROCEDURE insert_pais(
   	IN id VARCHAR(5),
   	IN nombre_pais VARCHAR(50)
   )
   BEGIN
   	INSERT INTO pais VALUES (id,nombre_pais);
   END $$
   
   DELIMITER ;
   
   CALL insert_pais('PER','PERU');
   
   
   ```

   

2. Insertar estado

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS insertar_estado;
   CREATE PROCEDURE insertar_estado(
   	IN nombre VARCHAR(25)
   )
   BEGIN 
   	INSERT INTO estado VALUES (NULL,nombre);
   END $$
   
   DELIMITER ;
   
   CALL insertar_estado('PERDIDO');
   ```

   

3. Actualizar estado

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS actualizar_estado;
   CREATE PROCEDURE actualizar_estado(
       IN id INT,
   	IN nombre_estado VARCHAR(25)
   )
   BEGIN 
   	UPDATE estado SET nombre = nombre_estado WHERE codigo_estado = id;
   END $$
   
   DELIMITER ;
   
   CALL actualizar_estado(4,'Perdido');
   ```

   

4. Contar empleados

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS conteo_empleado;
   CREATE PROCEDURE conteo_empleado(
   	OUT total INT
   )
   BEGIN 
   	SET total = (
       	SELECT COUNT(codigo_empleado) AS total_empleados
   		FROM empleado
       );
   END $$
   
   DELIMITER ;
   
   
   CALL conteo_empleado (@total);
   SELECT @total total_empleados;
   ```

   

5. Eliminar pais

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS eliminar_pais;
   CREATE PROCEDURE eliminar_pais(
   	IN id VARCHAR(5)
   )
   BEGIN
   	DELETE FROM pais WHERE codigo_pais = id;
   END $$
   
   DELIMITER ;
   
   CALL eliminar_pais ('PER');
   ```

   

6. Actualizar proveedor

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS actualizar_proveedor;
   CREATE PROCEDURE actualizar_proveedor(
     IN codigo INT(11),
     IN nombre_prov VARCHAR(50),
     IN email_prov VARCHAR(60)
   )
   BEGIN 
   	UPDATE proveedor 
   	SET 
   		codigo_proveedor = codigo,
   		nombre = nombre_prov,
   		email = email_prov		
   	    WHERE codigo_proveedor = codigo;
   END $$
   
   DELIMITER ;
   
   CALL actualizar_proveedor        (32,'BlossomBounty','blossombounty@example.net');
   ```

   

7. Eliminar proveedor

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS eliminar_proveedor;
   CREATE PROCEDURE eliminar_proveedor(
   	IN codigo VARCHAR(5)
   )
   BEGIN
   	DELETE FROM proveedor WHERE codigo_proveedor = codigo;
   END $$
   
   DELIMITER ;
   
   CALL eliminar_proveedor (32);
   ```

   

8. Clientes por país

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS clientes_pais;
   CREATE PROCEDURE clientes_pais()
   BEGIN 
       SELECT p.nombre_pais, COUNT(c.codigo_cliente) AS total_clientes
       FROM cliente c
       JOIN ciudad ci ON c.codigo_ciudad = ci.codigo_ciudad
       JOIN region r ON ci.codigo_region = r.codigo_region
       JOIN pais p ON r.codigo_pais = p.codigo_pais
       GROUP BY p.nombre_pais;
   END $$
   
   DELIMITER ;
   
   
   CALL clientes_pais ();
   
   ```

   

9. Pago promedio dependiendo del año

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS pago_promedio;
   CREATE PROCEDURE pago_promedio(
       IN anio INT
   )
   BEGIN 
       SELECT AVG(total_pago) AS pago_promedio
   	FROM pago
   	WHERE YEAR(fecha_pago) = anio;
   END $$
   
   DELIMITER ;
   
   CALL pago_promedio (2009);
   
   ```

   

10. Pedidos por estado

    ```mysql
    DELIMITER $$
    DROP PROCEDURE IF EXISTS pedidos_estado;
    CREATE PROCEDURE pedidos_estado ()
    BEGIN 
        SELECT e.nombre AS nombre_estado, COUNT(p.codigo_pedido) AS total_pedidos
        FROM pedido p
        JOIN estado e ON p.codigo_estado = e.codigo_estado
        GROUP BY p.codigo_estado, nombre_estado
        ORDER BY total_pedidos DESC;
    END $$
    
    DELIMITER ;
    
    
    CALL pedidos_estado ();
    
    ```

    
