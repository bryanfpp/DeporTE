# Consultoría Deportiva Pro

Aplicación web profesional de suscripción para servicios de consultoría y análisis deportivo. El proyecto está diseñado como una aplicación estática que usa Tailwind CSS y Supabase para guardar registros de usuarios.

## Estructura de carpetas

```
/App
  ├─ index.html
  ├─ admin.html
  └─ README.md
```

> Opción de escalabilidad: puede agregar carpetas `assets/css/`, `assets/js/` y `assets/img/` para separar estilos, lógica y recursos.

## Archivos principales

- `index.html`: formulario de suscripción para captar Nombre, Apellidos, Teléfono y Correo Electrónico.
- `admin.html`: listado de usuarios registrados y su estado (`pendiente` o `activo`).
- `README.md`: instrucciones de configuración y despliegue.

## Configuración de Supabase

1. Crea un proyecto en Supabase.
2. En el panel de Supabase, crea una tabla llamada `usuarios` con los campos:
   - `id` (UUID o integer, clave primaria)
   - `nombre` (text)
   - `segundo_nombre` (text)
   - `apellido_paterno` (text)
   - `apellido_materno` (text)
   - `apellidos` (text) -- para compatibilidad con el antiguo esquema
   - `telefono` (text)
   - `correo` (text)
   - `contrasena` (text)
   - `status` (text)
   - `created_at` (timestamp, valor por defecto `now()`)

   SQL para crear la tabla completa:
   ```sql
   CREATE TABLE usuarios (
     id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
     nombre TEXT NOT NULL,
     segundo_nombre TEXT,
     apellido_paterno TEXT NOT NULL,
     apellido_materno TEXT,
     apellidos TEXT,
     telefono TEXT NOT NULL,
     correo TEXT NOT NULL,
     contrasena TEXT NOT NULL,
     status TEXT NOT NULL DEFAULT 'pendiente',
     created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
   );
   ```3. Copia la URL del proyecto y la `anon key` desde la configuración de API.
4. Reemplaza los valores de `TU_SUPABASE_URL` y `TU_SUPABASE_ANON_KEY` en `index.html` y `admin.html`.

## Reglas recomendadas de RLS (Row Level Security)

Para que el formulario pueda insertar y el panel admin pueda leer, configura las políticas en Supabase:

- Habilita RLS en la tabla `usuarios`.
- Crea una política pública de inserción para el formulario.
- Crea una política de lectura para el panel admin si usas la misma `anon key`.

> Nota: Exponer `anon key` en un sitio estático es aceptable para lecturas/escrituras básicas si configuras políticas y no usas una `service_role` en el frontend.

## Despliegue en GitHub Pages

1. Crea un nuevo repositorio en GitHub.
2. Sube los archivos `index.html`, `admin.html` y `README.md` al repositorio.
3. En el repositorio, ve a `Settings > Pages`.
4. Selecciona la rama `main` o `master` y la carpeta `/(root)` como carpeta de origen.
5. Guarda los cambios. GitHub Pages publicará la aplicación en una URL del tipo `https://<usuario>.github.io/<repositorio>/`.

## Uso

- Accede a `index.html` para que los visitantes envíen su suscripción.
- Accede a `admin.html` para ver los registros y el estado de los usuarios.

## Escalabilidad

Para un proyecto más profesional, puedes extenderlo así:

- `assets/js/app.js` para lógica del formulario.
- `assets/js/admin.js` para lógica del panel.
- `assets/css/styles.css` para estilos personalizados.
- `public/` o `docs/` para contenido estático si usas GitHub Pages con carpetas específicas.

## Seguridad adicional

- Usa políticas RLS estrictas en Supabase.
- No mezcles `anon key` con credenciales de administrador.
- Para un backend más seguro, añade funciones serverless que actúen como proxy entre el frontend y Supabase.
