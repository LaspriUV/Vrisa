# Primera Entrega — Proyecto VRISA

## Introducción

La presente primera entrega del proyecto VRISA (Vigilancia de Riesgos e Inmisiones de Sustancias Atmosféricas) establece el punto de partida para el diseño y construcción de una plataforma orientada al monitoreo de la calidad del aire en la ciudad de Cali. Este sistema busca recopilar, almacenar y analizar mediciones provenientes de una red distribuida de estaciones ambientales, con el fin de identificar tendencias, detectar episodios críticos y ofrecer información confiable para la toma de decisiones (especialmente por parte de autoridades, investigadores y ciudadanía interesada).

En este documento se presentan los componentes fundamentales que dan forma al modelo inicial de datos y a la experiencia de usuario: un diagrama entidad–relación (ER) que representa los conceptos esenciales del dominio, su correspondiente esquema relacional con tipos de datos y restricciones, el diccionario de datos que describe cada elemento con precisión, bocetos preliminares de la interfaz de usuario (UI) y un diagrama de arquitectura de alto nivel que muestra la organización tecnológica planificada. Todo ello permitirá avanzar hacia una implementación consistente, escalable y alineada con los objetivos del proyecto.

---

## 1. Diagrama Entidad-Relación (ER)

En esta sección se presenta el diagrama entidad-relación que servirá como referencia para la gestión de datos dentro de la plataforma VRISA. Incluye entidades, atributos y relaciones fundamentales. Se utiliza notación **Chen** complementada con cardinalidad **crow’s foot** tal como indicado en clase.

[Diagrama ER VRISA](https://drive.google.com/file/d/1J7RlhjoAkN3S5g5riNK80eilfL36Rs0p/view?usp=sharing "Diagrama Entidad-Relación VRISA")

> El diagrama ER detalla las entidades principales del sistema, sus atributos clave y las relaciones entre ellas, estableciendo una base sólida para el diseño de la base de datos. Para accesar a el es por medio de un link de Drive abierto para todo mundo al diagrama en draw.io.

---

## 2. Esquema Relacional

A partir del ER se construye el esquema relacional correspondiente. Este permite una representación formal y operativa del almacenamiento.

### 2.1. Esquema Relacional

[Esquema Relacional VRISA](https://drive.google.com/file/d/11T5jr5fute78IJD1kf8KURjAXMPPOye6/view?usp=sharing "Esquema Relacional VRISA")

> El esquema relacional traduce las entidades y relaciones del diagrama ER en tablas con sus respectivos atributos, tipos de datos y restricciones, facilitando la implementación en un sistema de gestión de bases de datos relacional. Para accesar a el es por medio de un link de Drive abierto para todo mundo al esquema en draw.io.

### 2.2. Diccionario de Datos (Data Elements)

| **Tabla**                      | **Campo**                  | **Tipo**               | **Tamaño**          | **Descripción**                                                                                 |
|--------------------------------|----------------------------|------------------------|---------------------|-------------------------------------------------------------------------------------------------|
| **Usuario**                     | id                         | INT                    | -                   | Identificador único del usuario.                                                                |
|                                 | nombre                     | VARCHAR                | 50                  | Nombre del usuario.                                                                              |
|                                 | apellido                   | VARCHAR                | 50                  | Apellido del usuario.                                                                            |
|                                 | correo                     | VARCHAR                | 50                  | Correo electrónico del usuario.                                                                  |
|                                 | contrasena                 | CHAR                   | 30                  | Contraseña del usuario.                                                                          |
|                                 | tipo                       | INT                    | -                   | Tipo de usuario: 1=Ciudadano, 2=Investigador, 3=Admin institución, 4=Técnico, 5=Admin sistema.   |
| **Administrador de institución** | id_usuario                 | INT                    | -                   | Relación FK con Usuario.                                                                        |
|                                 | id_institucion             | INT                    | -                   | Relación FK con Institución.                                                                    |
| **Administrador de sistema**    | id_usuario                 | INT                    | -                   | Relación FK con Usuario.                                                                        |
|                                 | super_admin                | BOOLEAN                | -                   | Indica si el usuario es super administrador.                                                     |
| **Institución**                 | id                         | SMALLINT UNSIGNED      | -                   | Identificador único de la institución.                                                           |
|                                 | nombre                     | VARCHAR                | 80                  | Nombre de la institución.                                                                        |
|                                 | logo                       | VARCHAR                | 150                 | Logo de la institución.                                                                          |
|                                 | direccion                  | VARCHAR                | 120                 | Dirección de la institución.                                                                     |
|                                 | estado_validacion          | BOOLEAN                | -                   | Estado de validación.                                                                            |
| **Estación**                    | id                         | SMALLINT UNSIGNED      | -                   | Identificador único de la estación.                                                              |
|                                 | nombre                     | VARCHAR                | 60                  | Nombre de la estación.                                                                           |
|                                 | direccion                  | VARCHAR                | 120                 | Dirección de la estación.                                                                        |
|                                 | ubicacion_latitud          | DECIMAL                | (8,5)               | Latitud de la estación.                                                                          |
|                                 | ubicacion_longitud         | DECIMAL                | (8,5)               | Longitud de la estación.                                                                         |
|                                 | ubicacion                  | VARCHAR                | 80                  | Ubicación descriptiva.                                                                           |
| **Documento**                   | id                         | INT                    | -                   | Identificador único del documento.                                                               |
|                                 | fecha_de_creacion          | TIMESTAMP              | -                   | Fecha de creación del documento.                                                                 |
|                                 | tipo                       | VARCHAR                | 50                  | Tipo de documento (PDF, Excel, etc).                                                             |
|                                 | fecha_creacion             | DATE                   | -                   | Fecha de creación asociada.                                                                      |
|                                 | path                       | VARCHAR                | 255                 | Ruta del archivo en el servidor.                                                                 |
| **Alerta**                      | id                         | INT                    | -                   | Identificador único de la alerta.                                                                |
|                                 | tipo                       | VARCHAR                | 50                  | Tipo de alerta.                                                                                  |
|                                 | fecha_emision              | DATE                   | -                   | Fecha de emisión.                                                                                |
|                                 | nivel_contaminacion        | DECIMAL                | (5,2)               | Nivel de contaminación reportado.                                                                |
|                                 | description                | TEXT                   | -                   | Descripción de la alerta.                                                                        |
| **Sensor**                      | id                         | INT                    | -                   | Identificador único del sensor.                                                                  |
|                                 | tipo                       | VARCHAR                | 50                  | Tipo de sensor.                                                                                  |
|                                 | unidad_de_medida           | VARCHAR                | 50                  | Unidad de medida.                                                                                |
|                                 | fecha_de_calibracion       | TIMESTAMP              | -                   | Fecha de calibración.                                                                            |
| **Reporte**                     | id                         | INT                    | -                   | Identificador único del reporte.                                                                 |
|                                 | tipo                       | VARCHAR                | 50                  | Tipo de reporte.                                                                                 |
|                                 | fecha                      | TIMESTAMP              | -                   | Fecha del reporte.                                                                               |
|                                 | description                | TEXT                   | -                   | Descripción del reporte.                                                                         |
| **Estacion_reporte**            | id_estacion                | INT                    | -                   | Relación FK a Estación.                                                                          |
|                                 | id_reporte                 | INT                    | -                   | Relación FK a Reporte.                                                                           |
| **Medición**                    | id                         | INT                    | -                   | Identificador único de la medición.                                                              |
|                                 | fecha_hora                 | TIMESTAMP              | -                   | Fecha y hora de la medición.                                                                     |
|                                 | direccion_viento           | SMALLINT               | -                   | Dirección del viento (grados).                                                                   |
|                                 | velocidad_viento           | FLOAT                  | (3,1)               | Velocidad del viento (m/s).                                                                      |
|                                 | humedad                    | TINYINT                | -                   | Humedad relativa (%).                                                                            |
|                                 | temperatura                | FLOAT                  | (3,1)               | Temperatura (°C).                                                                                |
|                                 | cantidad_NO2               | FLOAT                  | (5,2)               | Nivel de NO₂.                                                                                    |
|                                 | cantidad_PM25              | FLOAT                  | (5,2)               | Nivel de PM2.5.                                                                                  |
|                                 | cantidad_SO2               | DECIMAL                | (5,2)               | Nivel de SO₂.                                                                                    |
|                                 | cantidad_O3                | DECIMAL                | (5,2)               | Nivel de O₃.                                                                                     |
|                                 | cantidad_CO                | DECIMAL                | (5,2)               | Nivel de CO.                                                                                     |

> El diccionario de datos proporciona una descripción detallada de cada tabla y campo en el esquema relacional, incluyendo tipos de datos y tamaños, facilitando la comprensión y uso adecuado de la base de datos.

---

## 3. Diseños de Interfaz de Usuario (UI)

Al tener claro el modelo de datos, se procede a definir los bocetos iniciales de la interfaz de usuario para las plataformas web y móvil de VRISA. Estos diseños buscan ofrecer una experiencia amigable y accesible para los diferentes tipos de usuarios identificados.

### 3.1 Bocetos de WEB UI

En esta sección se presentan los bocetos preliminares de la interfaz de usuario para la plataforma web de VRISA. Estos diseños buscan ofrecer una experiencia intuitiva y accesible para los diferentes tipos de usuarios identificados.

[Bocetos de WEB UI VRISA](https://drive.google.com/drive/folders/1XaKZ6ZbLvFvYchHPu7O0QKDdgxwN7Lzl?usp=sharing "Bocetos de WEB UI VRISA")

> En esta carpeta de Drive se encuentran los bocetos de la interfaz web, diseñados para facilitar la navegación y el acceso a la información relevante sobre la calidad del aire, son accesibles mediante el enlace proporcionado para cualquier persona.

### 3.2 Bocetos de Mobile UI

En esta sección se presentan los bocetos preliminares de la interfaz de usuario para la plataforma móvil de VRISA. Estos diseños están orientados a proporcionar una experiencia optimizada en dispositivos móviles, garantizando accesibilidad y usabilidad en cualquier momento y lugar.

[Bocetos de Mobile UI VRISA](https://drive.google.com/drive/folders/1KL-RSIuuT1NcMUXNF-IXeEdTrXYZlhhx?usp=sharing "Bocetos de Mobile UI VRISA")

> En esta carpeta de Drive se encuentran los bocetos de la interfaz móvil, diseñados para ofrecer una experiencia optimizada en dispositivos móviles, permitiendo a los usuarios acceder a la información sobre la calidad del aire desde cualquier lugar. Son accesibles mediante el enlace proporcionado para cualquier persona.

---

## 4. Diagrama de Arquitectura de Alto Nivel

![Diagrama de Arquitectura de Alto Nivel](images/Diagrama%20de%20arquitectura%20de%20alto%20nivel.jpeg "Diagrama de Arquitectura de Alto Nivel")

> En este diagrama se ilustra la estructura general del sistema VRISA, destacando los principales componentes y su interacción.

---

## Conclusiones

Con la entrega de estos elementos, se da inicio formal al desarrollo de VRISA, estableciendo las bases que permitirán evolucionar el sistema en iteraciones posteriores. El modelo ER y el esquema relacional garantizan que la información se almacene de forma estructurada, segura y sin redundancias innecesarias (evitando dolores de cabeza más adelante). Los prototipos de interfaz facilitan la visualización temprana de la experiencia del usuario, mientras que la arquitectura propuesta define cómo se comunicarán los diferentes componentes del sistema.

A partir de aquí, el equipo podrá avanzar hacia la implementación de la base de datos, la construcción de la API, las pruebas funcionales y, en general, el desarrollo de una plataforma capaz de generar valor ambiental y social. En últimas, esta entrega marca un paso importante hacia la creación de una herramienta que permita interpretar mejor la calidad del aire en Cali, fomentar la toma de decisiones informadas y promover prácticas sostenibles (que buena falta hacen).
