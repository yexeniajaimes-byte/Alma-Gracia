# ALMA Y GRACIA — Tienda de zapatos
**Eslogan:** Estilo con alma, elegancia con gracia.

Esta carpeta contiene una página web completamente editable, con catálogo público, filtros, lista de favoritos, botón de consulta por WhatsApp y un panel de administración para **agregar, editar, ocultar y eliminar zapatos, subir fotos y modificar precios**.

## IMPORTANTE: ¿cómo se guardan los productos?
La tienda utiliza **Supabase** para conservar el catálogo y las fotografías en línea. Una vez configurado, los productos nuevos que agregues aparecerán para todos los visitantes, sin tener que modificar el código ni volver a publicar archivos. Los visitantes no necesitan cuenta. Solo la persona autorizada puede ingresar al panel.

Sin Supabase la web muestra cuatro ejemplos (modo demostración) y **no permite administrar ni compartir cambios**. Esto evita aparentar que el catálogo ya está conectado cuando no lo está.

## PASO 1 — Abrir y ver la web
1. Descarga y descomprime la carpeta.
2. Instala Visual Studio Code: https://code.visualstudio.com/
3. Abre la carpeta `alma-y-gracia` desde VS Code.
4. Instala la extensión **Live Server** en VS Code (opcional) y haz clic derecho en `index.html` > **Open with Live Server**. También puedes abrir `index.html` con tu navegador para una vista preliminar.

### Archivos que puedes editar
- `index.html`: textos, eslogan, títulos y secciones.
- `style.css`: colores, tamaños y presentación. Los colores principales están al inicio, en `:root`.
- `config.js`: enlace de tu base de datos, WhatsApp e Instagram.
- `app.js`: funciones del catálogo y administración.
- `assets/zapato.svg`: imagen genérica cuando no se ha subido una foto.

## PASO 2 — Crear base de datos y almacenamiento (Supabase)
1. Crea una cuenta en https://supabase.com y un proyecto nuevo.
2. En el proyecto entra a **SQL Editor**, crea una consulta, pega todo el contenido de `supabase.sql` y ejecútala.
3. En **Project Settings > API** (la ubicación exacta puede cambiar según la interfaz) copia la **Project URL** y la clave **anon / publishable**.
4. Abre `config.js`, pega esos dos valores entre comillas y guarda. **Nunca** uses una clave `service_role` ni una clave secreta del servidor en este archivo.
5. Abre **Authentication > Users** y crea la cuenta que utilizarás para administrar con tu correo y una contraseña segura. Puedes usar **Add user** si está disponible.
6. Copia el **User UID** de esa cuenta.
7. Vuelve a **SQL Editor** y ejecuta lo siguiente, reemplazando el texto por el UID real:

```sql
insert into public.admin_users (user_id)
values ('TU-UUID-DE-USUARIO');
```

8. El SQL ya configura la tabla `products`, sus permisos (RLS), y el bucket público `product-images`. Las imágenes se pueden ver públicamente, pero solo la cuenta registrada en `admin_users` puede subirlas.

**Seguridad:** la autenticación y los permisos se comprueban en Supabase, no solo ocultando botones en la página. No compartas tu contraseña. Si quieres otro administrador, crea otra cuenta y añade su UID a `admin_users`.

## PASO 3 — Personalizar WhatsApp y redes
En `config.js` cambia:

```js
whatsapp: "573001234567", // ejemplo: reemplaza por tu número real
instagram: "https://www.instagram.com/tu_tienda/"
```

El teléfono debe incluir el indicativo `57` para Colombia y no contener espacios ni `+`.

## PASO 4 — Probar el panel
1. Ejecuta el sitio en Live Server.
2. Haz clic en **Administrar** (arriba a la derecha).
3. Inicia sesión con tu correo y contraseña creados en Supabase.
4. Completa nombre, categoría, precio, tallas, descripción y fotografía de tu zapato.
5. Haz clic en **Guardar producto**. La imagen se sube a Supabase Storage y el producto queda guardado en la base de datos.
6. Prueba **Editar** y **Eliminar** en la lista inferior.
7. Si desmarcas **Mostrar en el catálogo**, el producto se oculta al público sin borrarse.

## PASO 5 — Compartir el sitio con cualquier persona
### Publicación gratuita con GitHub Pages
1. Crea una cuenta en https://github.com/.
2. Crea un repositorio nuevo, por ejemplo `alma-y-gracia` (público).
3. Sube **todos los archivos y la carpeta `assets`**, manteniendo la misma estructura.
4. Entra a **Settings > Pages** y configura **Deploy from a branch**, rama `main`, carpeta `/(root)`. Guarda.
5. GitHub te mostrará una dirección parecida a: `https://TUUSUARIO.github.io/alma-y-gracia/`.
6. Abre ese enlace desde otro teléfono para comprobar que funcione. Ya puedes enviarlo por WhatsApp o ponerlo en tu biografía de Instagram.

Al agregar productos desde el panel publicado, se actualizan en Supabase y otros visitantes podrán verlos al cargar o recargar la página. **No necesitas subir de nuevo el proyecto a GitHub por cada producto.**

Si después deseas un dominio como `almaygracia.com`, puedes comprarlo y conectarlo a GitHub Pages.

## Recomendaciones importantes
- No compartas tu usuario y contraseña de administración.
- Mantén la clave `service_role` y otras claves secretas fuera de `config.js`.
- La clave pública (`anon`/`publishable`) sí está diseñada para usarse en el navegador; la seguridad depende de las políticas RLS de `supabase.sql`.
- Para publicar, debes conservar `config.js` (con tus claves públicas, no privadas).
- Máximo por imagen: 5 MB. Formatos permitidos: JPG, PNG y WebP.
- El catálogo inicial muestra productos de ejemplo solo antes de conectar Supabase; reemplázalos por tus productos reales.
- El botón de WhatsApp **no procesa pagos**: genera una consulta con los productos seleccionados. Para pagos en línea necesitarás una integración adicional.
- El costo del sitio puede ser gratuito en los planes gratuitos de los servicios dentro de sus límites de uso; revisa condiciones vigentes de Supabase y GitHub.
- Este primer modelo no incluye gestión de inventario por talla, usuarios compradores ni pasarela de pagos.
- La imagen del zapato es un recurso ilustrativo de demostración; al subir tus fotos se verán las reales.
