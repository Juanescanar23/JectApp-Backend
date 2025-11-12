# JectApp - Backend

API REST construida con Express y MongoDB (Mongoose) para gestionar los recursos principales de **JectApp**: usuarios, vehículos, rutas, empresas, barrios, archivos, correo y comunicación en tiempo real mediante Socket.IO.

## Pila tecnológica

- Node.js 14+ con Express como framework HTTP.
- MongoDB como base de datos documental, accesible mediante Mongoose.
- Autenticación con JWT, Google Sign-In y validaciones personalizadas.
- Manejo de archivos con `express-fileupload` y persistencia local en `uploads/`.
- Envío de correos usando Nodemailer.
- Socket.IO listo para habilitar mensajería en tiempo real.

## Características destacadas

- CRUD de usuarios, vehículos, rutas, barrios, empresas y marcadores.
- Búsqueda centralizada por colecciones (`/busqueda`).
- Carga y servicio de imágenes con control de colecciones y extensiones permitidas.
- Middleware de autenticación y autorización centralizado en `middlewares/`.
- Cliente estático servido desde `cliente/` para pruebas rápidas.

## Requisitos previos

1. **Node.js** ≥ 14 y **npm** instalados.
2. **MongoDB** accesible localmente (`mongodb://localhost:27017/jectappDB`) o mediante una URL externa.
3. Variables/secretos definidos en `config/config.js` (seed JWT, credenciales OAuth y cualquier otro secreto que necesites).

## Instalación y puesta en marcha

```bash
git clone https://github.com/Juanescanar23/JectApp-Backend.git
cd JectApp-Backend
npm install
```

1. Ajusta `config/config.js` con tu propio `SEED`, `CLIENT_ID` y `GOOGLE_SECRET` si usarás autenticación federada.
2. Inicia tu instancia de MongoDB local (`mongod`) o actualiza la URL en `app.js`.
3. Ejecuta el servidor:

```bash
npm start
```

El servicio queda disponible en `http://localhost:3000`. Puedes probar los endpoints con herramientas como Postman o Thunder Client. El cliente estático servido desde `cliente/` puede ayudar a validar el flujo básico de login.

## Scripts disponibles

| Script      | Descripción                                    |
|-------------|------------------------------------------------|
| `npm start` | Levanta el servidor Express en modo producción |
| `npm test`  | Placeholder (personalízalo si agregas pruebas) |

> Sugerencia: agrega un script `dev` con `nodemon app.js` si necesitas recarga en caliente durante el desarrollo.

## Arquitectura y directorios clave

- `app.js`: punto de entrada. Configura CORS, body-parser, archivos estáticos, Socket.IO (comentado) y registra todas las rutas.
- `routes/`: separa cada dominio (usuarios, vehículos, rutas, uploads, imágenes, sockets, barrios, empresas, marcadores, mail, login). Cada archivo maneja validaciones y controladores específicos.
- `models/`: esquemas Mongoose para usuarios, vehículos, rutas, barrios, empresas, marcadores, etc.
- `middlewares/`: lógica de autenticación JWT, verificación de roles y validaciones reutilizables.
- `config/`: llaves y secretos compartidos. Puedes migrarlo a variables de entorno para despliegues productivos.
- `uploads/` y `assets/`: almacenamiento de archivos e imágenes expuestas públicamente.
- `cliente/`: micro front-end que consume la API; útil como referencia de integración.

## Flujo básico de petición

1. **Autenticación**: los clientes se autentican vía `/login` (credenciales o Google). Se genera un JWT firmado con `SEED`.
2. **Autorización**: los endpoints protegidos usan middleware para validar token y, cuando aplica, el rol del usuario.
3. **Persistencia**: los modelos Mongoose definen validaciones, índices únicos (`mongoose-unique-validator`) y relaciones con otras colecciones.
4. **Archivos**: `/upload/:tipo/:id` maneja la subida, validando tipo y formato. `/imagenes/:tipo/:img` sirve los archivos con fallback.
5. **Comunicación en tiempo real**: la configuración Socket.IO está lista en `sockets/` para habilitar eventos cuando se requiera (solo descomenta en `app.js`).

## Despliegue

- Define tus variables sensibles como variables de entorno o secret managers y cárgalas antes de iniciar el proceso (`process.env`).
- Configura la URL de MongoDB con credenciales gestionadas (`mongodb+srv://...`).
- Usa un process manager (PM2, systemd, Docker) para producción, exponiendo el puerto 3000 o el que configures en `server.listen`.

## Próximos pasos recomendados

- Añadir pruebas automatizadas (unitarias e integración) para los servicios críticos.
- Crear documentación Swagger/Redoc a partir de las rutas actuales.
- Contenerizar la aplicación para despliegues consistentes.

Con esto tienes una referencia completa para instalar, operar y extender el backend de **JectApp**. ¡Listo para producción tras configurar tus credenciales!
