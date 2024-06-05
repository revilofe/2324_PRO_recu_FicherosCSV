# Actividad: Recuperación módulo de programación

**ID actividad: Recu.1**

**Agrupamiento de la actividad**: Individual

---

### 1. Descripción:

La actividad consiste en procesar *un conjunto de archivos* en formato csv, que están almacenados en una carpeta.

Los archivos contienen las calificaciones de los instrumentos con los que los alumnos del ciclo se han evaluado. El procesamiento consisten en calcular las calificaciones de los criterios de evaluación y resultados de aprendizaje en base a las calificaciones de los instrumentos y las ponderaciones de estos establecidas en el archivo csv.

> nota: Analiza con cuidado el formato del archivo base y su formato, puesto que cualquier cambio tendrá que ser compatible para que no haya problemas al intentar leer el csv desde cualquier aplicación que soporta este archivo.

El formato de los archivos será similar al siguiente:

![](./assets/csv.png)

1. id de la unidad (Un)
2. id del resultado de aprendizaje (RA)
3. id de los criterios de evaluación (CE). Normalmente se trabajará con la letra después del punto.
4. ids de los CE afectados (para los que se calculará la nota) por el instrumento.
5. Porcentajes que pesa cada CE, en base a los cuales se obtiene la nota total del resultado de aprendizaje.
6. id y nombre del alumnado
7. descripción del RA
8. descripción de cada uno de los CE
9. id y descripción de los instrumentos usados para calcular la nota de los CE. Ten en cuenta que a continuación está el peso en porcentaje, que tiene cada instrumento.
10. notas de los instrumentos.
11. área de calculo, en el que se tendrán que calcular las notas de los CE y RA.

![](./assets/csv2.png)

1. notas de los RA
2. notas de los CE

> nota: Cualquier duda sobre el formato, aclárala antes de continuar.

### 2. Opciones de ejecución del programa.

Estas opciones se le tienen que indicar al programa al ejecutarlo:

- `-pi`: indica al programa que se procese los archivos y la carpeta origen en la que se encuentran los archivos a procesar. Si no se indica nada, se asume la carpeta actual. Como resultado del procesamiento deberá mostrar por consola según el formato indicado en 3.2.1 las calificaciones de los elementos (RA, CE) del módulo por alumno y actualizar el archivo con las calificaciones de los elementos (RA, CE) del módulo por alumno. 
    `$ recu -pi /home/usuario/csvfiles`
- `-mo`: complementa a `-pi`, e indica al programa el `módulo`. Esta opción indica al programa que tiene que procesar los archivos de calificaciones de un módulo concreto. Si no se indica nada, se asume el módulo de programación, que se identifica como `PRO`. Si se indica solo, se mostrará por consola las calificaciones de los elementos (RA, CE) del módulo por alumno.
    `$ recu -pi /home/usuario/csvfiles -mo PRO -bd`
- `-bd`: complementa a `-pi`, e indica al programa que la información que se muestra por pantalla tiene que quedar almacenada en base de datos. Si ya existe, tendrá que actualizarse.   
    `$ recu -pi /home/usuario/csvfiles -bd`
- `-bd q`: indica al programa que consulte la información que hay en la base de datos. Mostrando por consola según el formato indicado en 3.2.1. Esta opción es excluyente con cualquier otra opción.
    `$ recu -bd q`
- `-bd qi`: indica al programa que consulte la información que hay en la base de datos. Mostrando esta a través de la interface gráfica, según el formato indicado en 3.2.2. Esta opción es excluyente con cualquier otra opción.
    `$ recu -bd qi`
- `-bd d`: indica al programa que borre la información que hay en la base de datos, quedando limpia de toda información. Esta opción es excluyente con cualquier otra opción.  
    `$ recu -bd d`

> nota: es posible la combinación de los parámetros anteriores siempre y cuando tengan sentido, por ejemplo puedo indicarle:
> `recu -mo PRO -pi /home/usuario/csvfiles -bd`

### 3. Lectura/Escritura de archivo y Salidas del programa.

#### 3.1. Lectura y escritura

La ruta a la carpeta en la que se encuentran los archivos csv se pasará con la opción `-pi` cuando se ejecute el programa. El programa deberá procesar todos los archivos que encuentre en la carpeta, y se asumen que pertenecen a un módulo concreto.

> Nota: Para el procesamiento de los archivos no se podrá usar ninguna biblioteca adicional que no sea la clase `FILE` y demás clases que permite leer y escribir en archivos.

Tras cada procesamiento, el archivo que se está procesando, se modificará para dejar constancia de las calificacioens que se han obtenido en los distintos elementos (RAs, CEs). Inicialmente se podrá pasar vacío, pero en posteriores llamadas es posible que las calificaciones ya estén establecidas y, por tanto, se tendrán que actualizar con los nuevos valores calculados.

![](./assets/csv2.png)

#### 3.2. Salidas

Una vez se haya procesado la información mediante la opcion `-pi`, el programa tiene que actuar de la siguiente forma:  

##### 3.2.1 Consola

El programa mostrará como salida, en formato tabla, las calificaciones del módulo teniendo en cuenta que ésta calificación se calcula en base a la nota acumulada de todos los archivos procesados, es decir, la calificación de los resultados de aprendizaje, las calificaciones de los criterios de evaluación. En la tabla tiene que aparecer el id de cada uno de los elementos (Módulo, RAs, CEs), la descripción y finalmente la calificación. Las tablas serán similares al siguiente diseño:

![](assets/tablaConsola.png)


##### 3.2.2 Interface gráfica

De igual forma, el programa mostrará en la interfaz gráfica las calificaciones, en un diseño similar al siguiente:

![](assets/gui.png)

En el que se podrá seleccionar los alumnos y mostrará un resumen de sus notas, tal y como se ve en la imagen anterior.

### 4. Base de datos.

El programa deberá almacenar en base de datos los resultados de procesar los archivos.

#### 4.1. Opciones de ejecución del programa relacionadas con la base de datos.

El programa, cuando reciba la opción `-bd`, almacenará en base de datos los resultados de procesar los archivos, quedando constancia de las calificaciones del alumnado para los distintos elementos que forman parte de estas. (Módulo, RAs, CEs).

Conforme se vayan procesando archivos, siempre que se indique la opción `-bd`, se tendrán que incorporar los nuevos elementos y actualizar la calificación del módulo.

La opción `-bd q` permitirá consultlar la información de la base de datos, mostrando por consola según el formato indicado en 3.2.1.

La opción `-bd qi` permitirá consultlar la información de la base de datos, mostrando esta a través de la interface gráfica, según el formato indicado en 3.2.2.

La opción `-bd d` permitirá borrar la información de la base de datos, quedando limpia de toda información.

![](./assets/csv2.png)

En el que se podrá seleccionar los alumnos y mostrará un resumen de sus notas, tal y como se ve en 3.2.2.

#### 4.2. Modelo de Base de datos

Para la base de datos podéis utilizar H2 con el siguiente modelo simplificado, no óptimo, ni normalizado, pero suficiente para almacenar la información necesaria y dedicaros a mostrarnos vuestros conocimientos de programación.

```
CREATE TABLE Alumno (
	idAlumno VARCHAR(3) NOT NULL,
	nombre VARCHAR(255) NOT NULL,
	PRIMARY KEY (idAlumno)
);

CREATE TABLE AlumnoRA (
	idAlumno VARCHAR(3) NOT NULL,
	idModulo VARCHAR(3) NOT NULL,
	idRA INT NOT NULL,
	desc VARCHAR(255),
	porcentaje NUMBER,
	nota NUMBER,
	PRIMARY KEY (idAlumno, idModulo, idRA)
);

CREATE TABLE AlumnoCE (
	idAlumno VARCHAR(3) NOT NULL,
	idModulo VARCHAR(3) NOT NULL,
	idRA INT NOT NULL,
	idCE VARCHAR(1) NOT NULL,
	desc VARCHAR(255),
	porcentaje NUMBER,
	nota NUMBER,
	PRIMARY KEY (idAlumno, idModulo, idRA, idCE)
);

ALTER TABLE AlumnoRA
ADD FOREIGN KEY (idAlumno)
REFERENCES Alumno(idAlumno);

ALTER TABLE AlumnoCE
ADD FOREIGN KEY (idAlumno)
REFERENCES Alumno(idAlumno);

ALTER TABLE AlumnoCE
ADD FOREIGN KEY (idAlumno, idModulo, idRA)
REFERENCES AlumnoRA(idAlumno, idModulo, idRA);
```

### 5. Recomendaciones

1. Trabajar sobre el parseo de argumentos pasados al ejecutar el programa.   
2. Trabajar sobre el formato de los datos y el parseo de estos.   
3. Separa la entrada de datos, procesamiento de datos, salida de datos.   
4. Usa patrones de diseño, jerarquía de clases.   
5. Asigna una única responsabilidad por clase, y por métodos.   
6. Pon nombres coherentes y adecuados a la responsabilidad que realiza la clase (Sustantivos) /métodos (verbos).    
7. Construir una estructura de datos/clases que soporte la información en memoria y facilite realizar las tareas de procesamiento de la información.   
8. Trabajar por partes las distintas salidas:
   a. salida a ficheros.
   b. salida a consola.
   c. salida a base de datos.
   d. interface gráfica.
9. Documentar y comentar.    
10. Genera el ejecutable.    
11. Crear las capturas de pantallas de aquellas ejecuciones y opciones que funcionen.    
12. No olvides crear el ejecutable del programa.
13. Haz una buena gestión de errores, mostrando mensajes claros y concisos.

### 6. Recursos

* Apuntes dado en clase
* Recursos vistos en clase.

### 7. Evaluación y calificación

Se trabajan prácticamente todas las unidades, con su RA y CE asociados, aunque sólo se evaluan unidades 7 y 9. No obstante no se puede olvidar nada de lo aprendido en las unidades anteriores, ya que forman parte de la base de conocimientos necesarios para poder realizar la actividad.

#### 7.1. Unidades: Criterios de evaluación

##### Unidad 4.

a) Entender los fundamentos de la POO
i) Haciendo uso del IDE, b) escribir programa simples en los que:
c) Se instancian objetos a partir de clases predefinidas, h) haciendo uso de constructores
d) Se utilicen métodos, f) haciendo uso de parámetros en la llamada, y propiedades de los objetos.
e) Se llamen a métodos estáticos.
g) Se incorporan y utilizan librerías.

##### Unidad 5.

a) Entender la sintaxis, estructura y componentes típicos de una clase.
b,c) Definir clases, con sus e) constructores, d) propiedades y métodos, i) métodos estáticos, en los que se utilicen g) los mecanismos para controlar la visibilidad de clase y miembros.
f) Desarrollar programas que instancien y utilicen objetos de las clases creadas anteriormente.
h) Definir y utilizar clases heredadas e j) interfaces.
k) Crear y utilizar librerías.

##### Unidad 6.

a) Entender los conceptos de herencia, superclase y subclase.
g) Crear programas en los que e) implementen y utilicen jerarquías de clases, para ello d) crear clases heredadas que sobrescriban la implementación de métodos de la superclase, c) reconociendo la incidencia de los constructores en la herencia y b) utilizando modificadores para bloquear y forzar la herencia de clases y métodos.
f) Se han probado y depurado las jerarquías de clases.
h) Se ha comentado y documentado el código.

##### Unidad 7.

a) Se ha utilizado la consola para realizar operaciones de entrada y salida de información, y b) Se han aplicado formatos en la visualización de la información.
c) Conocer las posibilidades de entrada/salida del lenguaje y las librerías asociadas, d) utilizado ficheros para almacenar y recuperar información y e) que utilicen diversos métodos de acceso al contenido de los ficheros.
f) Se han utilizado el IDE para crear interfaces gráficos de usuario simples, en las que se g) se controlen eventos, y h) se utilice la gui para la entrada y salida de información.

##### Unidad 9.

a) Se han identificado las características y métodos de acceso a sistemas gestores de bases de datos relacionales.
b) Se han creado programas en las que crear conexiones con bases de datos, para c) almacenar información en bases de datos, d) recuperar y mostrar información almacenada en bases de datos, efectuar e) borrados y modificaciones sobre la información almacenada y f) que ejecuten consultas sobre bases de datos.
g) Se han creado aplicaciones para posibilitar la gestión de información presente en bases de datos relacionales.

#### 7.2. Rúbrica

Se construirá en base a la información anterior.

**Conlleva presentación**: SI

### 8. Entrega

> **La actividad tiene que cumplir las condiciones de entrega para poder ser calificada. En caso de no cumplirlas podría calificarse como no entregada.**

* **Conlleva la entrega de URL a repositorio:** El contenido se entregará en un repositorio github, y en caso de creerlo necesario se trabajará por proyectos dejando constancia de las acciones realizas según cada uno de los perfiles de los usuarios del grupo.
* **Conlleva la entrega de capturas de pantallas de las distintas opciones:** Se entregará un documento (markdown en la raíz del repositorio con nombre Recu_PRO_<Iniciales>.md) que contendrán capturas de pantalla de las distintas ejecuciones del programa, mostrando aquellas que funcionan correctamente. *Solo se pasará a evaluar aquellas que tengan una captura de pantalla sobre su funcionamiento*.
* **Conlleva la entrega un ejecutable del programa:** Se tendrá que entregar un enlace al ejecutable del programa, de forma que se pueda probar sin necesidad de ejecutar el IDE. Este enlace estará en un documento (markdown en la raíz del repositorio).