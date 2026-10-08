# 🛒 POS – Backend (NestJS)

API REST de un **Punto de Venta (POS)** para una tienda de ropa y accesorios. Gestiona categorías, productos con inventario, ventas, cupones de descuento y subida de imágenes.

Está construida con **NestJS** y **TypeScript**, usa **PostgreSQL** como base de datos a través de **TypeORM**, y **Cloudinary** para almacenar las imágenes de los productos.

> 🔗 El frontend de este proyecto está en [pos-frontend](https://github.com/SuemyDzib/pos-frontend).

## ✨ Características

- **CRUD de categorías**, con opción de obtener una categoría junto con sus productos.
- **CRUD de productos** con paginación (`take` / `skip`) y filtro por categoría.
- **Control de inventario**: al registrar una venta se descuenta el stock, y si se elimina la venta, el stock se restaura.
- **Registro de ventas dentro de una transacción de base de datos**, para que no se guarden ventas incompletas si algún producto no existe o no tiene suficiente inventario.
- **Cupones de descuento** con porcentaje y fecha de expiración; se validan al aplicarlos y al registrar la venta.
- **Consulta de ventas por fecha**.
- **Subida de imágenes** de productos a Cloudinary.
- **Validación de datos** con `class-validator` y mensajes de error en español.
- **Seeder** con categorías y productos de ejemplo, incluyendo sus imágenes.

## 🛠️ Tecnologías

- [NestJS 10](https://nestjs.com/) con [TypeScript](https://www.typescriptlang.org/)
- [PostgreSQL](https://www.postgresql.org/) con [TypeORM](https://typeorm.io/)
- [class-validator](https://github.com/typestack/class-validator) y [class-transformer](https://github.com/typestack/class-transformer)
- [Cloudinary](https://cloudinary.com/) y [Multer](https://github.com/expressjs/multer) para imágenes
- [date-fns](https://date-fns.org/) para el manejo de fechas
- [Jest](https://jestjs.io/) para pruebas

## 📋 Requisitos

- [Node.js](https://nodejs.org/) 18 o superior
- Una base de datos PostgreSQL (local o en un servicio como [Render](https://render.com/), [Neon](https://neon.tech/) o [Supabase](https://supabase.com/))
- Una cuenta de [Cloudinary](https://cloudinary.com/)

## 🚀 Instalación y uso

1. Clona el repositorio:

   ```bash
   git clone https://github.com/SuemyDzib/pos-backend.git
   cd pos-backend
   ```

2. Instala las dependencias:

   ```bash
   npm install
   ```

3. Crea un archivo `.env` a partir del ejemplo y completa tus datos:

   ```bash
   cp .env.example .env
   ```

4. (Opcional) Carga los datos de ejemplo:

   ```bash
   npm run seed
   ```

5. Inicia el servidor en modo desarrollo:

   ```bash
   npm run start:dev
   ```

   La API quedará disponible en `http://localhost:3001`.

> ⚠️ **El seeder borra la base de datos completa** antes de insertar los datos de ejemplo. Úsalo solo en desarrollo o en una base de datos nueva.

> La conexión a PostgreSQL usa SSL (`ssl: true` en `src/config/typeorm.config.ts`), que es lo que requieren la mayoría de los servicios en la nube. Si usas una base de datos local sin SSL, cambia ese valor a `false`.

### Variables de entorno

| Variable                | Descripción                                        |
| ----------------------- | -------------------------------------------------- |
| `PORT`                  | Puerto del servidor (opcional, por defecto `3001`) |
| `DATABASE_HOST`         | Host de PostgreSQL                                 |
| `DATABASE_PORT`         | Puerto de PostgreSQL (normalmente `5432`)          |
| `DATABASE_USER`         | Usuario de la base de datos                        |
| `DATABASE_PASS`         | Contraseña de la base de datos                     |
| `DATABASE_NAME`         | Nombre de la base de datos                         |
| `CLOUDINARY_NAME`       | *Cloud name* de tu cuenta de Cloudinary            |
| `CLOUDINARY_API_KEY`    | API key de Cloudinary                              |
| `CLOUDINARY_API_SECRET` | API secret de Cloudinary                           |

### Scripts

| Comando              | Descripción                                          |
| -------------------- | ---------------------------------------------------- |
| `npm run start:dev`  | Inicia el servidor con recarga automática            |
| `npm run build`      | Compila el proyecto en la carpeta `dist/`            |
| `npm run start:prod` | Ejecuta la versión compilada                         |
| `npm run seed`       | Reinicia la base de datos y carga datos de ejemplo   |
| `npm run lint`       | Analiza y corrige el código con ESLint               |
| `npm test`           | Ejecuta las pruebas unitarias                        |

### Ejemplos

Crear un cupón:

```http
POST /coupons
Content-Type: application/json

{
  "name": "DESCUENTO10",
  "percentage": 10,
  "expirationDate": "2026-12-31"
}
```

Registrar una venta:

```http
POST /transactions
Content-Type: application/json

{
  "total": 98,
  "coupon": "DESCUENTO10",
  "contents": [
    { "productId": 1, "quantity": 2, "price": 49 }
  ]
}
```

### Imágenes

Las imágenes de los productos de ejemplo se sirven como archivos estáticos desde `public/img`, por ejemplo `http://localhost:3001/img/1.jpg`. Las imágenes que se suben desde el panel de administración se guardan en Cloudinary.

## 📝 Notas

- La API no incluye autenticación, por lo que todos los endpoints son públicos. Es un proyecto de práctica; para producción convendría proteger las rutas de administración.
- TypeORM tiene `synchronize: true`, lo que crea y actualiza las tablas automáticamente a partir de las entidades. Es cómodo en desarrollo, pero en producción se recomienda usar migraciones.

## 👤 Autor

Desarrollado por **Suemy Dzib** – [@SuemyDzib](https://github.com/SuemyDzib) a través del curso de Udemy impartido por Juan de la Torre.