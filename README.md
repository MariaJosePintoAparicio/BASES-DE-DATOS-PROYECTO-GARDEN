# PROYECTO BASES DE DATOS

Crear base de datos:

```sql
CREATE DATABASE garden;
```

Crear tablas de la base de datos:

```sql
CREATE TABLE IF NOT EXISTS `garden`.`gama_product` (
    `gama_product_id` VARCHAR(50) NOT NULL,
    `description` TEXT NULL,
    `description_html` TEXT NULL, 
    `image` VARCHAR(256) NULL, 
    PRIMARY KEY (`gama_product_id`)) ENGINE = InnoDB;
     	+------------------+
		| Tables_in_garden |
		+------------------+
		| gama_product     |
		+------------------+
		
CREATE TABLE IF NOT EXISTS `garden`.`product` ( 
    `product_code` VARCHAR(15) NOT NULL, 
    `product_name` VARCHAR(70) NULL, 
    `description` TEXT NULL, 
    `stock_amount` SMALLINT(6) NULL,
    `price_sell` DECIMAL(15) NULL, 
    `gama` VARCHAR(50) NOT NULL, 
    `height` SMALLINT(6) NULL,
    `width` SMALLINT(6) NULL, 
    `length` SMALLINT(6) NULL, 
    `weight` SMALLINT(6) NULL, 
    PRIMARY KEY (`product_code`), 
    INDEX `fk_product_gama_product_idx` (`gama` ASC) VISIBLE, 
    UNIQUE INDEX `product_code_UNIQUE` (`product_code` ASC) VISIBLE,
    CONSTRAINT `fk_product_gama_product` 
    FOREIGN KEY (`gama`) 
    REFERENCES `garden`.`gama_product` (`gama_product_id`)) 
   ENGINE = InnoDB;
    
   	 	+------------------+
		| Tables_in_garden |
		+------------------+
		| gama_product     |
		| product          |
		+------------------+
		
CREATE TABLE IF NOT EXISTS `garden`.`provider` ( 
    `provider_id` INT NOT NULL, 
    `provider_name` VARCHAR(50) NULL, 
    `provider_surname` VARCHAR(50) NULL, 
    PRIMARY KEY (`provider_id`)) ENGINE = InnoDB;	
   		+------------------+
		| Tables_in_garden |
		+------------------+
		| gama_product     |
		| product          |
		| provider         |
		+------------------+
		
CREATE TABLE IF NOT EXISTS `garden`.`provider_product` (
  `provider_id` INT NOT NULL,
  `new_price` DECIMAL(15) NULL,
  `old_price` DECIMAL(15) NULL,
  `product_code` VARCHAR(15) NOT NULL,
  INDEX `fk_provider_product_provider1_idx` (`provider_id` ASC) VISIBLE,
  INDEX `fk_provider_product_product1_idx` (`product_code` ASC) VISIBLE,
  CONSTRAINT `pk_provider_product` PRIMARY KEY (`provider_id`, `product_code`),
  CONSTRAINT `fk_provider_product_provider1`
    FOREIGN KEY (`provider_id`)
    REFERENCES `garden`.`provider` (`provider_id`),
  CONSTRAINT `fk_provider_product_product1`
    FOREIGN KEY (`product_code`)
    REFERENCES `garden`.`product` (`product_code`))
ENGINE = InnoDB;	
		+------------------+
		| Tables_in_garden |
		+------------------+
		| gama_product     |
		| product          |
		| provider         |
		| provider_product |
		+------------------+

CREATE TABLE IF NOT EXISTS `garden`.`rol` (
  `rol_id` INT(11) NOT NULL AUTO_INCREMENT,
  `rol_name` VARCHAR(50) NOT NULL,
  `showProducts` TINYINT NULL DEFAULT 1,
  `actived` TINYINT NULL DEFAULT 1,
  `created_at` DATETIME NULL DEFAULT NOW(),
  `updated_at` DATETIME NULL DEFAULT NOW(),
  PRIMARY KEY (`rol_id`))
ENGINE = InnoDB;

		+------------------+
		| Tables_in_garden |
		+------------------+
		| gama_product     |
		| product          |
		| provider         |
		| provider_product |
		| rol              |
		+------------------+

CREATE TABLE IF NOT EXISTS `garden`.`country` (
  `country_id` INT(10) NOT NULL AUTO_INCREMENT,
  `country_name` VARCHAR(45) NULL,
  PRIMARY KEY (`country_id`))
ENGINE = InnoDB;	

		+------------------+
		| Tables_in_garden |
		+------------------+
		| country          |
		| gama_product     |
		| product          |
		| provider         |
		| provider_product |
		| rol              |
		+------------------+

CREATE TABLE IF NOT EXISTS `garden`.`region` (
  `region_id` INT(10) NOT NULL AUTO_INCREMENT,
  `region_name` VARCHAR(45) NULL,
  `country_id` INT(10) NOT NULL,
  PRIMARY KEY (`region_id`),
  INDEX `fk_region_country1_idx` (`country_id` ASC) VISIBLE,
  CONSTRAINT `fk_region_country1`
  FOREIGN KEY (`country_id`)
  REFERENCES `garden`.`country` (`country_id`))
ENGINE = InnoDB;
        +------------------+
        | Tables_in_garden |
        +------------------+
        | country          |
        | gama_product     |
        | product          |
        | provider         |
        | provider_product |
        | region           |
        | rol              |
        +------------------+
        
        
CREATE TABLE IF NOT EXISTS `garden`.`city` (
  `city_id` INT(10) NOT NULL AUTO_INCREMENT,
  `city_name` VARCHAR(45) NULL,
  `postal_code` VARCHAR(45) NULL,
  `region_id` INT(10) NOT NULL,
  PRIMARY KEY (`city_id`),
  INDEX `fk_city_region1_idx` (`region_id` ASC) VISIBLE,
  CONSTRAINT `fk_city_region1`
  FOREIGN KEY (`region_id`)
  REFERENCES `garden`.`region` (`region_id`))
ENGINE = InnoDB;

        +------------------+
        | Tables_in_garden |
        +------------------+
        | city             |
        | country          |
        | gama_product     |
        | product          |
        | provider         |
        | provider_product |
        | region           |
        | rol              |
        +------------------+


CREATE TABLE IF NOT EXISTS `garden`.`office` (
  `office_id` VARCHAR(10) NOT NULL,
  `office_phone_number` VARCHAR(45) NULL,
  `address_line_1` VARCHAR(50) NULL,
  `address_line_2` VARCHAR(50) NULL,
  `city_id` INT(10) NOT NULL,
  `main_office_id` VARCHAR(10) NULL DEFAULT NULL,
  PRIMARY KEY (`office_id`),
  INDEX `fk_office_city1_idx` (`city_id` ASC) VISIBLE,
  INDEX `fk_office_office1_idx` (`main_office_id` ASC) VISIBLE,
  CONSTRAINT `fk_office_city1`
    FOREIGN KEY (`city_id`)
    REFERENCES `garden`.`city` (`city_id`),
  CONSTRAINT `fk_office_office1`
    FOREIGN KEY (`main_office_id`)
    REFERENCES `garden`.`office` (`office_id`))
ENGINE = InnoDB;
        +------------------+
        | Tables_in_garden |
        +------------------+
        | city             |
        | country          |
        | gama_product     |
        | office           |
        | product          |
        | provider         |
        | provider_product |
        | region           |
        | rol              |
        +------------------+
        
CREATE TABLE IF NOT EXISTS `garden`.`employee` (
  `employee_id` INT(11) NOT NULL,
  `employee_first_name` VARCHAR(50) NULL,
  `employee_last_name` VARCHAR(50) NULL DEFAULT NULL,
  `employee_first_surname` VARCHAR(50) NULL,
  `employee_last_surname` VARCHAR(50) NULL,
  `employee_extension` VARCHAR(45) NULL,
  `employee_email` VARCHAR(100) NULL,
  `boss_id` INT(11) NULL,
  `rol_id` INT(11) NOT NULL,
  `actived` TINYINT NULL DEFAULT 1,
  `created_at` DATETIME NULL DEFAULT NOW(),
  `updated_at` DATETIME NULL DEFAULT NOW(),
  `office_id` VARCHAR(10) NOT NULL,
  PRIMARY KEY (`employee_id`),
  INDEX `fk_employee_employee1_idx` (`boss_id` ASC) VISIBLE,
  INDEX `fk_employee_rol1_idx` (`rol_id` ASC) VISIBLE,
  INDEX `fk_employee_office1_idx` (`office_id` ASC) VISIBLE,
  CONSTRAINT `fk_employee_employee1`
    FOREIGN KEY (`boss_id`)
    REFERENCES `garden`.`employee` (`employee_id`),
  CONSTRAINT `fk_employee_rol1`
    FOREIGN KEY (`rol_id`)
    REFERENCES `garden`.`rol` (`rol_id`),
  CONSTRAINT `fk_employee_office1`
    FOREIGN KEY (`office_id`)
    REFERENCES `garden`.`office` (`office_id`))
ENGINE = InnoDB;

        +------------------+
        | Tables_in_garden |
        +------------------+
        | city             |
        | country          |
        | employee         |
        | gama_product     |
        | office           |
        | product          |
        | provider         |
        | provider_product |
        | region           |
        | rol              |
        +------------------+


CREATE TABLE IF NOT EXISTS `garden`.`customer` (
  `customer_id` INT NOT NULL,
  `customer_name` VARCHAR(50) NULL,
  `customer_surname` VARCHAR(50) NULL,
  `credit_limit` DECIMAL(15) NULL,
  `employee_id` INT(11) NULL DEFAULT NULL,
  `customer_email` VARCHAR(45) NULL,
  PRIMARY KEY (`customer_id`),
  INDEX `fk_customer_employee1_idx` (`employee_id` ASC) VISIBLE,
  CONSTRAINT `fk_customer_employee1`
    FOREIGN KEY (`employee_id`)
    REFERENCES `garden`.`employee` (`employee_id`))
ENGINE = InnoDB;

        +------------------+
        | Tables_in_garden |
        +------------------+
        | city             |
        | country          |
        | customer         |
        | employee         |
        | gama_product     |
        | office           |
        | product          |
        | provider         |
        | provider_product |
        | region           |
        | rol              |
        +------------------+

CREATE TABLE IF NOT EXISTS `garden`.`address` (
    ->   `address_id` INT NOT NULL AUTO_INCREMENT,
    ->   `address_line_1` VARCHAR(250) NULL,
    ->   `address_line_2` VARCHAR(250) NULL,
    ->   `address_type` VARCHAR(45) NULL,
    ->   `city_id` INT(10) NOT NULL,
    ->   `provider_id` INT NULL DEFAULT NULL,
    ->   `customer_id` INT NULL DEFAULT NULL,
    ->   `employee_id` INT(11) NULL DEFAULT NULL,
    ->   PRIMARY KEY (`address_id`),
    ->   INDEX `fk_address_city1_idx` (`city_id` ASC) VISIBLE,
    ->   INDEX `fk_address_provider1_idx` (`provider_id` ASC) VISIBLE,
    ->   INDEX `fk_address_customer1_idx` (`customer_id` ASC) VISIBLE,
    ->   INDEX `fk_address_employee1_idx` (`employee_id` ASC) VISIBLE,
    ->   CONSTRAINT `fk_address_city1`
    ->     FOREIGN KEY (`city_id`)
    ->     REFERENCES `garden`.`city` (`city_id`),
    ->   CONSTRAINT `fk_address_provider1`
    ->     FOREIGN KEY (`provider_id`)
    ->     REFERENCES `garden`.`provider` (`provider_id`),
    ->   CONSTRAINT `fk_address_customer1`
    ->     FOREIGN KEY (`customer_id`)
    ->     REFERENCES `garden`.`customer` (`customer_id`),
    ->   CONSTRAINT `fk_address_employee1`
    ->     FOREIGN KEY (`employee_id`)
    ->     REFERENCES `garden`.`employee` (`employee_id`))
    -> ENGINE = InnoDB;
            +------------------+
            | Tables_in_garden |
            +------------------+
            | address          |
            | city             |
            | country          |
            | customer         |
            | employee         |
            | gama_product     |
            | office           |
            | product          |
            | provider         |
            | provider_product |
            | region           |
            | rol              |
            +------------------+
            
CREATE TABLE IF NOT EXISTS `garden`.`customer_contact` (
  `customer_contact_id` INT NOT NULL AUTO_INCREMENT,
  `cc_first_name` VARCHAR(45) NULL,
  `cc_last_name` VARCHAR(45) NULL DEFAULT NULL,
  `cc_first_surname` VARCHAR(45) NULL,
  `cc_last_surname` VARCHAR(45) NULL DEFAULT NULL,
  `customer_contact_type` VARCHAR(45) NULL,
  `customer_id` INT NOT NULL,
  PRIMARY KEY (`customer_contact_id`),
  INDEX `fk_customer_contact_customer1_idx` (`customer_id` ASC) VISIBLE,
  CONSTRAINT `fk_customer_contact_customer1`
    FOREIGN KEY (`customer_id`)
    REFERENCES `garden`.`customer` (`customer_id`))
ENGINE = InnoDB;
        +------------------+
        | Tables_in_garden |
        +------------------+
        | address          |
        | city             |
        | country          |
        | customer         |
        | customer_contact |
        | employee         |
        | gama_product     |
        | office           |
        | product          |
        | provider         |
        | provider_product |
        | region           |
        | rol              |
        +------------------+
        
 CREATE TABLE IF NOT EXISTS `garden`.`phone_number` (
  `phone_number_id` INT NOT NULL AUTO_INCREMENT,
  `phone_number` VARCHAR(45) NULL,
  `fax` VARCHAR(15) NULL,
  `phone_number_prefix` VARCHAR(45) NULL DEFAULT NULL,
  `phone_number_type` VARCHAR(45) NULL DEFAULT NULL,
  `created_at` DATETIME NULL DEFAULT NOW(),
  `updated_at` DATETIME NULL DEFAULT NOW(),
  `who_is` VARCHAR(45) NULL,
  `office_id` VARCHAR(10) NULL DEFAULT NULL,
  `employee_employee_id` INT(11) NULL DEFAULT NULL,
  `provider_id` INT NULL DEFAULT NULL,
  `customer_contact_id` INT NULL DEFAULT NULL,
  PRIMARY KEY (`phone_number_id`),
  INDEX `fk_phone_number_office1_idx` (`office_id` ASC) VISIBLE,
  INDEX `fk_phone_number_employee1_idx` (`employee_employee_id` ASC) VISIBLE,
  INDEX `fk_phone_number_provider1_idx` (`provider_id` ASC) VISIBLE,
  INDEX `fk_phone_number_customer_contact1_idx` (`customer_contact_id` ASC) VISIBLE,
  CONSTRAINT `fk_phone_number_office1`
    FOREIGN KEY (`office_id`)
    REFERENCES `garden`.`office` (`office_id`),
  CONSTRAINT `fk_phone_number_employee1`
    FOREIGN KEY (`employee_employee_id`)
    REFERENCES `garden`.`employee` (`employee_id`),
  CONSTRAINT `fk_phone_number_provider1`
    FOREIGN KEY (`provider_id`)
    REFERENCES `garden`.`provider` (`provider_id`),
  CONSTRAINT `fk_phone_number_customer_contact1`
    FOREIGN KEY (`customer_contact_id`)
    REFERENCES `garden`.`customer_contact` (`customer_contact_id`))
ENGINE = InnoDB;

        +------------------+
        | Tables_in_garden |
        +------------------+
        | address          |
        | city             |
        | country          |
        | customer         |
        | customer_contact |
        | employee         |
        | gama_product     |
        | office           |
        | phone_number     |
        | product          |
        | provider         |
        | provider_product |
        | region           |
        | rol              |
        +------------------+
        
 CREATE TABLE IF NOT EXISTS `garden`.`order` (
  `order_code` INT NOT NULL,
  `date_order` DATE NULL,
  `date_waiting` DATE NULL,
  `date_deliver` DATE NULL,
  `comments` TEXT NULL,
  `status` VARCHAR(15) NULL,
  `customer_id` INT NOT NULL,
  PRIMARY KEY (`order_code`),
  INDEX `fk_order_customer1_idx` (`customer_id` ASC) VISIBLE,
  CONSTRAINT `fk_order_customer1`
    FOREIGN KEY (`customer_id`)
    REFERENCES `garden`.`customer` (`customer_id`))
ENGINE = InnoDB;
        +------------------+
        | Tables_in_garden |
        +------------------+
        | address          |
        | city             |
        | country          |
        | customer         |
        | customer_contact |
        | employee         |
        | gama_product     |
        | office           |
        | order            |
        | phone_number     |
        | product          |
        | provider         |
        | provider_product |
        | region           |
        | rol              |
        +------------------+
 
 CREATE TABLE IF NOT EXISTS `garden`.`order_detail` (
  `amount` INT(11) NULL,
  `price_unit` DECIMAL(15) NULL,
  `line_numer` SMALLINT(6) NULL,
  `order_code` INT NOT NULL,
  `product_code` VARCHAR(15) NOT NULL,
  INDEX `fk_order_detail_order1_idx` (`order_code` ASC) VISIBLE,
  INDEX `fk_order_detail_product1_idx` (`product_code` ASC) VISIBLE,
  CONSTRAINT `pk_order_detail` PRIMARY KEY (`product_code`, `order_code`),
  CONSTRAINT `fk_order_detail_order1`
    FOREIGN KEY (`order_code`)
    REFERENCES `garden`.`order` (`order_code`),
  CONSTRAINT `fk_order_detail_product1`
    FOREIGN KEY (`product_code`)
    REFERENCES `garden`.`product` (`product_code`))
ENGINE = InnoDB;
        +------------------+
        | Tables_in_garden |
        +------------------+
        | address          |
        | city             |
        | country          |
        | customer         |
        | customer_contact |
        | employee         |
        | gama_product     |
        | office           |
        | order            |
        | order_detail     |
        | phone_number     |
        | product          |
        | provider         |
        | provider_product |
        | region           |
        | rol              |
        +------------------+

CREATE TABLE IF NOT EXISTS `garden`.`pay` (
  `id_transaction` VARCHAR(50) NOT NULL,
  `date_pay` DATE NULL,
  `total` DECIMAL(15) NULL,
  `method_pay` VARCHAR(45) NULL,
  `customer_id` INT NOT NULL,
  PRIMARY KEY (`id_transaction`),
  INDEX `fk_pay_customer1_idx` (`customer_id` ASC) VISIBLE,
  CONSTRAINT `fk_pay_customer1`
    FOREIGN KEY (`customer_id`)
    REFERENCES `garden`.`customer` (`customer_id`))
ENGINE = InnoDB;
        +------------------+
        | Tables_in_garden |
        +------------------+
        | address          |
        | city             |
        | country          |
        | customer         |
        | customer_contact |
        | employee         |
        | gama_product     |
        | office           |
        | order            |
        | order_detail     |
        | pay              |
        | phone_number     |
        | product          |
        | provider         |
        | provider_product |
        | region           |
        | rol              |
        +------------------+
```

### CONSULTAS

### Consultas sobre una tabla

1. Devuelve un listado con el código de oficina y la ciudad donde hay oficinas.

   ```sql
   
   ```

   

2. Devuelve un listado con la ciudad y el teléfono de las oficinas de España.

   ```
   
   ```

   

3. Devuelve un listado con el nombre, apellidos y email de los empleados cuyo
  jefe tiene un código de jefe igual a 7.

  ```
  
  ```

  

4. Devuelve el nombre del puesto, nombre, apellidos y email del jefe de la
  empresa.

  ```
  
  ```

  

5. Devuelve un listado con el nombre, apellidos y puesto de aquellos
  empleados que no sean representantes de ventas.

  ```
  
  ```

  

6. Devuelve un listado con el nombre de los todos los clientes españoles.

   ```
   
   ```

   

7. Devuelve un listado con los distintos estados por los que puede pasar un
  pedido.

  ```
  
  ```

  

8. Devuelve un listado con el código de cliente de aquellos clientes que
  realizaron algún pago en 2008. Tenga en cuenta que deberá eliminar
  aquellos códigos de cliente que aparezcan repetidos. Resuelva la consulta:
  • Utilizando la función YEAR de MySQL.
  • Utilizando la función DATE_FORMAT de MySQL.
  • Sin utilizar ninguna de las funciones anteriores.

  ```
  
  ```

  

9. Devuelve un listado con el código de pedido, código de cliente, fecha
  esperada y fecha de entrega de los pedidos que no han sido entregados a
  tiempo.

  ```
  
  ```

  

10. Devuelve un listado con el código de pedido, código de cliente, fecha
    esperada y fecha de entrega de los pedidos cuya fecha de entrega ha sido al
    menos dos días antes de la fecha esperada.
    • Utilizando la función ADDDATE de MySQL.
    • Utilizando la función DATEDIFF de MySQL.
    • ¿Sería posible resolver esta consulta utilizando el operador de suma + o
    resta -?

    ```
    
    ```

    

11. Devuelve un listado de todos los pedidos que fueron rechazados en 2009.

    ```
    
    ```

    

12. Devuelve un listado de todos los pedidos que han sido entregados en el
    mes de enero de cualquier año.

    ```
    
    ```

    

13. Devuelve un listado con todos los pagos que se realizaron en el
    año 2008 mediante Paypal. Ordene el resultado de mayor a menor.

    ```
    
    ```

    

14. Devuelve un listado con todas las formas de pago que aparecen en la
    tabla pago. Tenga en cuenta que no deben aparecer formas de pago
    repetidas.

    ```
    
    ```

    

15. Devuelve un listado con todos los productos que pertenecen a la
    gama Ornamentales y que tienen más de 100 unidades en stock. El listado
    deberá estar ordenado por su precio de venta, mostrando en primer lugar
    los de mayor precio.

    ```
    
    ```

    

16. Devuelve un listado con todos los clientes que sean de la ciudad de Madrid y
    cuyo representante de ventas tenga el código de empleado 11 o 30.

    ```
    
    ```

    

    ### Consultas multitabla (Composición interna)

    Resuelva todas las consultas utilizando la sintaxis de SQL1 y SQL2. Las consultas con
    sintaxis de SQL2 se deben resolver con INNER JOIN y NATURAL JOIN.

    

17. Obtén un listado con el nombre de cada cliente y el nombre y apellido de su
   representante de ventas.

   ```
   
   ```

   

18. Muestra el nombre de los clientes que hayan realizado pagos junto con el
   nombre de sus representantes de ventas.

   ```
   
   ```

   

19. Muestra el nombre de los clientes que no hayan realizado pagos junto con
   el nombre de sus representantes de ventas.

   ```
   
   ```

   

20. Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus
   representantes junto con la ciudad de la oficina a la que pertenece el
   representante.

   ```
   
   ```

   

21. Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre
   de sus representantes junto con la ciudad de la oficina a la que pertenece el
   representante.

   ```
   
   ```

   

22. Lista la dirección de las oficinas que tengan clientes en Fuenlabrada.

    ```
    
    ```

    

23. Devuelve el nombre de los clientes y el nombre de sus representantes junto
   con la ciudad de la oficina a la que pertenece el representante.

   ```
   
   ```

   

24. Devuelve un listado con el nombre de los empleados junto con el nombre
   de sus jefes.

   ```
   
   ```

   

25. Devuelve un listado que muestre el nombre de cada empleados, el nombre
   de su jefe y el nombre del jefe de sus jefe.

   ```
   
   ```

   

26. Devuelve el nombre de los clientes a los que no se les ha entregado a
    tiempo un pedido.

    ```
    
    ```

    

27. Devuelve un listado de las diferentes gamas de producto que ha comprado
    cada cliente.

    ```
    
    ```

    

    ### Consultas multitabla (Composición externa)

    Resuelva todas las consultas utilizando las cláusulas LEFT JOIN, RIGHT JOIN, NATURAL
    LEFT JOIN y NATURAL RIGHT JOIN.

    

    

28. Devuelve un listado que muestre solamente los clientes que no han
   realizado ningún pago.

   ```
   
   ```

   

29. Devuelve un listado que muestre solamente los clientes que no han
   realizado ningún pedido.

   ```
   
   ```

   

30. Devuelve un listado que muestre los clientes que no han realizado ningún
   pago y los que no han realizado ningún pedido.

   ```
   
   ```

   

31. Devuelve un listado que muestre solamente los empleados que no tienen
   una oficina asociada.

   ```
   
   ```

   

32. Devuelve un listado que muestre solamente los empleados que no tienen un
   cliente asociado.

   ```
   
   ```

   

33. Devuelve un listado que muestre solamente los empleados que no tienen un
   cliente asociado junto con los datos de la oficina donde trabajan.

   ```
   
   ```

   

34. Devuelve un listado que muestre los empleados que no tienen una oficina
   asociada y los que no tienen un cliente asociado.

   ```
   
   ```

   

35. Devuelve un listado de los productos que nunca han aparecido en un
   pedido.

   ```
   
   ```

   

36. Devuelve un listado de los productos que nunca han aparecido en un
   pedido. El resultado debe mostrar el nombre, la descripción y la imagen del
   producto.

   ```
   
   ```

   

37. Devuelve las oficinas donde no trabajan ninguno de los empleados que
    hayan sido los representantes de ventas de algún cliente que haya realizado
    la compra de algún producto de la gama Frutales.

    ```
    
    ```

    

38. Devuelve un listado con los clientes que han realizado algún pedido pero no
    han realizado ningún pago.

    ```
    
    ```

    

39. Devuelve un listado con los datos de los empleados que no tienen clientes
    asociados y el nombre de su jefe asociado.

    ```
    
    ```

    ### Consultas resumen

40. ¿Cuántos empleados hay en la compañía?

    ```
    
    ```

    

41. ¿Cuántos clientes tiene cada país?

    ```
    
    ```

    

42. ¿Cuál fue el pago medio en 2009?

    ```
    
    ```

    

43. ¿Cuántos pedidos hay en cada estado? Ordena el resultado de forma
   descendente por el número de pedidos.

   ```
   
   ```

   

44. Calcula el precio de venta del producto más caro y más barato en una
   misma consulta.

   ```
   
   ```

   

45. Calcula el número de clientes que tiene la empresa.

    ```
    
    ```

    

46. ¿Cuántos clientes existen con domicilio en la ciudad de Madrid?

    ```
    
    ```

    

47. ¿Calcula cuántos clientes tiene cada una de las ciudades que empiezan
   por M?

   ```
   
   ```

   

48. Devuelve el nombre de los representantes de ventas y el número de clientes
   al que atiende cada uno.

   ```
   
   ```

   

49. Calcula el número de clientes que no tiene asignado representante de
    ventas.

    ```
    
    ```

    

50. Calcula la fecha del primer y último pago realizado por cada uno de los
    clientes. El listado deberá mostrar el nombre y los apellidos de cada cliente.

    ```
    
    ```

    

51. Calcula el número de productos diferentes que hay en cada uno de los
    pedidos.

    ```
    
    ```

    

52. Calcula la suma de la cantidad total de todos los productos que aparecen en
    cada uno de los pedidos.

    ```
    
    ```

    

53. Devuelve un listado de los 20 productos más vendidos y el número total de
    unidades que se han vendido de cada uno. El listado deberá estar ordenado
    por el número total de unidades vendidas.

    ```
    
    ```

    

54. La facturación que ha tenido la empresa en toda la historia, indicando la
    base imponible, el IVA y el total facturado. La base imponible se calcula
    sumando el coste del producto por el número de unidades vendidas de la
    tabla detalle_pedido. El IVA es el 21 % de la base imponible, y el total la
    suma de los dos campos anteriores.

    ```
    
    ```

    

55. La misma información que en la pregunta anterior, pero agrupada por
    código de producto.

    ```
    
    ```

    

56. La misma información que en la pregunta anterior, pero agrupada por
    código de producto filtrada por los códigos que empiecen por OR.

    ```
    
    
    ```

    

57. Lista las ventas totales de los productos que hayan facturado más de 3000
    euros. Se mostrará el nombre, unidades vendidas, total facturado y total
    facturado con impuestos (21% IVA).

    ```
    
    ```

    

58. Muestre la suma total de todos los pagos que se realizaron para cada uno
    de los años que aparecen en la tabla pagos.

    ### Subconsultas

    #### Con operadores básicos de comparación

    ```
    
    ```

    

59. Devuelve el nombre del cliente con mayor límite de crédito.

    ```
    
    ```

    

60. Devuelve el nombre del producto que tenga el precio de venta más caro.

    ```
    
    ```

    

61. Devuelve el nombre del producto del que se han vendido más unidades.
   (Tenga en cuenta que tendrá que calcular cuál es el número total de
   unidades que se han vendido de cada producto a partir de los datos de la
   tabla detalle_pedido)

   ```
   
   ```

   

62. Los clientes cuyo límite de crédito sea mayor que los pagos que haya
   realizado. (Sin utilizar INNER JOIN).

   ```
   
   ```

   

63. Devuelve el producto que más unidades tiene en stock.K

    ```
    
    ```

    

64. Devuelve el producto que menos unidades tiene en stock.

    ```
    
    ```

    

65. Devuelve el nombre, los apellidos y el email de los empleados que están a
   cargo de Alberto Soria.

   ```
   
   ```

   ### Subconsultas con ALL y ANY

66. Devuelve el nombre del cliente con mayor límite de crédito.

    ```
    
    ```

    

67. Devuelve el nombre del producto que tenga el precio de venta más caro.

    ```
    
    ```

    

68. Devuelve el producto que menos unidades tiene en stock.

    ```
    
    ```

    ### Subconsultas con IN y NOT IN

69. Devuelve el nombre, apellido1 y cargo de los empleados que no
    representen a ningún cliente.

    ```
    
    ```

    

70. Devuelve un listado que muestre solamente los clientes que no han
    realizado ningún pago.

    ```
    
    ```

    

71. Devuelve un listado que muestre solamente los clientes que sí han realizado
    algún pago.

    ```
    
    ```

    

72. Devuelve un listado de los productos que nunca han aparecido en un
    pedido.

    ```
    
    ```

    

73. Devuelve el nombre, apellidos, puesto y teléfono de la oficina de aquellos
    empleados que no sean representante de ventas de ningún cliente.

    ```
    
    ```

    

74. Devuelve las oficinas donde no trabajan ninguno de los empleados que
    hayan sido los representantes de ventas de algún cliente que haya realizado
    la compra de algún producto de la gama Frutales.

    ```
    
    ```

    

75. Devuelve un listado con los clientes que han realizado algún pedido pero no
    han realizado ningún pago.

    ```
    
    ```

    

### Subconsultas con EXISTS y NOT EXISTS

18. Devuelve un listado que muestre solamente los clientes que no han
    realizado ningún pago.

    ```
    
    ```

    
19. Devuelve un listado que muestre solamente los clientes que sí han realizado
    algún pago.

    ```
    
    ```

    
20. Devuelve un listado de los productos que nunca han aparecido en un
    pedido.

    ```
    
    ```

    
21. Devuelve un listado de los productos que han aparecido en un pedido
    alguna vez.

    ```
    
    ```

    

    #### Subconsultas correlacionadas

    ### Consultas variadas
22. Devuelve el listado de clientes indicando el nombre del cliente y cuántos
   pedidos ha realizado. Tenga en cuenta que pueden existir clientes que no
   han realizado ningún pedido.

   ```
   
   ```

   
23. Devuelve un listado con los nombres de los clientes y el total pagado por
   cada uno de ellos. Tenga en cuenta que pueden existir clientes que no han
   realizado ningún pago.

   ```
   
   ```

   
24. Devuelve el nombre de los clientes que hayan hecho pedidos en 2008
   ordenados alfabéticamente de menor a mayor.

   ```
   
   ```

   
25. Devuelve el nombre del cliente, el nombre y primer apellido de su
   representante de ventas y el número de teléfono de la oficina del
   representante de ventas, de aquellos clientes que no hayan realizado ningún
   pago.

   ```
   
   ```

   
26. Devuelve el listado de clientes donde aparezca el nombre del cliente, el
   nombre y primer apellido de su representante de ventas y la ciudad donde
   está su oficina.

   ```
   
   ```

   
27. Devuelve el nombre, apellidos, puesto y teléfono de la oficina de aquellos
   empleados que no sean representante de ventas de ningún cliente.

   ```
   
   ```

   

28. Devuelve un listado indicando todas las ciudades donde hay oficinas y el
   número de empleados que tiene.

   ```
   
   ```

   