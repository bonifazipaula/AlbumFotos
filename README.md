### Álbum de Fotos Compartido Temporal

Aplicación web diseñada para crear álbumes de fotos colaborativos orientados a eventos. Permite que múltiples usuarios contribuyan con sus fotografías de forma rápida, sin necesidad de registro previo.

### Funcionalidades

* **Creación de álbumes:** Formulario para nombre, descripción, fecha del evento y contraseña opcional. Generación de enlace único para compartir.
* **Acceso seguro:** Acceso público mediante enlace, con validación de contraseña en caso de ser requerida.
* **Gestión de imágenes:** Subida de fotos (JPG, PNG, WebP) con límite de 10 MB por archivo.
* **Galería:** Visualización responsive en formato grid, con vista detallada (lightbox) de cada imagen.
* **Estadísticas:** Contador de fotos subidas y espacio total consumido (en MB).
* **Descarga colaborativa:** Funcionalidad para descargar todas las fotos del álbum comprimidas en un archivo ZIP.
* **Expiración automática:** Eliminación integral (fotos y metadatos) 30 días después de la fecha del evento.

## Resolución Técnica

### Base de Datos y Entorno

* **Tecnología:** Se utiliza **MongoDB** como base de datos NoSQL, gestionada mediante el ODM **Mongoose**.
* **Infraestructura:** Para el entorno de desarrollo y gestión de la base de datos, se ha implementado **Docker** a través de `docker-compose.yml`, garantizando un entorno aislado y persistente para el motor de base de datos.
* **Modelado:** Se estructuró la información en 3 modelos principales para optimizar el rendimiento:
* `Album`: Metadatos del evento y configuración.
* `Photo`: Referencias y metadatos de las imágenes.
* `Image`: Datos binarios (Buffers) de la imagen y su respectivo thumbnail.



> **Optimización:** La separación de `Image` permite cargar la galería sin transferir el peso total de los archivos binarios, evitando respuestas ineficientes del servidor.

### Desafíos NoSQL

Al trabajar con un modelo NoSQL, la integridad referencial no es nativa. Para garantizar la limpieza total al expirar un álbum, se implementó una **lógica de eliminación en cascada** a nivel de aplicación:

1. Recorrido de los documentos asociados en la colección `Photo`.
2. Eliminación de los archivos binarios correspondientes en `Image`.
3. Limpieza final del documento raíz en la colección `Album`.

---

### Cómo levantar el proyecto

Para ejecutar el entorno de base de datos de manera sencilla, se incluye un script de automatización:

1. **Levantar base de datos:** `./up.sh` (o `sudo docker compose up -d`)
2. **Instalar dependencias:** `pnpm install`
3. **Ejecutar aplicación:** `pnpm dev`
4. **Visualización de datos:** Se puede utilizar **MongoDB Compass** conectándose a `mongodb://localhost:27017` para inspeccionar las colecciones en tiempo real.
