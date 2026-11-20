# M Collections

Sitio web tipo catálogo para M Collections. Permite explorar y buscar productos, filtrarlos por sección y consultar detalles. Los pedidos se inician por WhatsApp. El panel de administración permite gestionar productos y secciones.

## Estructura

- `index.html`: entrada para GitHub Pages si se publica la raíz de la rama; carga los recursos de `site/assets/`.
- `site/`: código y archivos estáticos del catálogo.
- `site/index.html`: página principal.
- `site/assets/app.js`: interfaz, autenticación y conexión con Firebase.
- `site/assets/styles.css`: estilos.
- `site/assets/firebase-config.js`: configuración pública de la aplicación web de Firebase.
- `.github/workflows/pages.yml`: publica `site/` automáticamente al hacer push a `main`.

No se necesita compilar ni instalar dependencias locales. El navegador carga Firebase desde Google y Tailwind CSS y las fuentes desde sus CDNs.

## Publicación en GitHub Pages

1. En GitHub, abre **Settings → Pages** del repositorio y selecciona **GitHub Actions** como origen de publicación.
2. Sube estos cambios a la rama `main` o ejecuta manualmente **Deploy to GitHub Pages** en la pestaña **Actions**.
3. Espera a que termine el flujo y abre la URL que aparece en **Settings → Pages** o en el despliegue del flujo.

El flujo publica el contenido de `site/`, por lo que la página y sus recursos mantienen rutas válidas tanto en una URL de proyecto como en un dominio propio. Si GitHub Pages está configurado para publicar la raíz de la rama `main`, `index.html` también sirve el catálogo desde la raíz y carga sus recursos desde `site/assets/`.

## Configuración de Firebase

La tienda usa Firebase Authentication para el acceso al panel y Cloud Firestore para leer y guardar las colecciones `products` y `sections`.

1. Crea un proyecto de Firebase y registra una aplicación web.
2. Habilita el proveedor **Email/Password** en Authentication y crea únicamente las cuentas de administrador necesarias.
3. Crea una base de datos de Cloud Firestore.
4. Copia la configuración de la aplicación web a `site/assets/firebase-config.js`. No incluyas contraseñas, claves privadas, tokens ni credenciales de cuentas en el repositorio.
5. Configura las reglas de Firestore para permitir la lectura pública del catálogo y limitar las escrituras a usuarios autenticados. Las reglas son el control de acceso real a los datos: protege las cuentas de administrador y revísalas antes de publicar.
6. En Authentication, agrega el dominio de GitHub Pages del sitio a los dominios autorizados.

La configuración web de Firebase se entrega al navegador y no es una contraseña. Aun así, no publiques credenciales privadas; protege Firestore con reglas y restringe las cuentas con acceso al panel. En la aplicación actual, cualquier usuario autenticado puede abrir el panel, así que no crees cuentas para visitantes.

## Desarrollo local

Sirve `site/` con un servidor HTTP estático y abre la dirección local indicada por el servidor. No abras `index.html` directamente como archivo: la aplicación usa módulos JavaScript y Firebase, que necesitan un origen HTTP.
