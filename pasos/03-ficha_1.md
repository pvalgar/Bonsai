# Paso 3: Ficha: formulario web para crear y editar bonsais

## Objetivo
Una página (`web/ficha.html`) para crear y editar la ficha de cada bonsai en Supabase, con inicio de sesión. Sin sesión no se ve ni se guarda nada (RLS).

## Pasos
1. Copiar `ficha.html` a `Bonsai/web/` y subirlo al repositorio `pvalgar/Bonsai` (GitHub Pages lo sirve en https://pvalgar.github.io/Bonsai/web/ficha.html).
2. Crear en Supabase > Authentication > Users el usuario único (correo y contraseña), si no existe. Comprobar que "Allow new users to sign up" sigue desactivado.
3. (Opcional) Insertar en la tabla `zonas` las zonas reales (terraza, invernadero...), para poder asignarlas desde el formulario.
4. Abrir la página, entrar y crear un bonsai.

## Qué hace
- Listado con búsqueda por código o nombre; los bonsais de baja salen en gris.
- Formulario: código (propone el siguiente libre), nombre, especie, estilo, zona actual, origen, árbol madre (solo para Acodo, Esqueje e Injerto), adquisición, precio, notas y baja.
- Especies: se elige de la tabla `especies`; "Nueva especie…" permite crearla con su clasificación completa.
- Zona: al cambiarla se cierra la zona anterior (`hasta` = hoy) y se abre la nueva en `bonsai_zona`. Si se corrige el mismo día, se modifica en lugar de crear historial.
- Baja: al desmarcar "activo" piden fecha y tipo de baja (catálogo `tipos_baja`) y qué ocurrió.
- Validaciones en el formulario y mensajes claros para código o nombre repetidos.

## Comprobación
- [ ] Sin sesión, la página solo muestra el inicio de sesión.
- [ ] Crear un bonsai con una especie existente y comprobar la fila en Supabase > Table Editor > `bonsais`.
- [ ] Crear una especie nueva desde el formulario y usarla.
- [ ] Repetir un código y un nombre: debe salir el aviso de duplicado.
- [ ] Crear un acodo con árbol madre elegido de la lista.
- [ ] Asignar zona, cambiarla y ver dos filas en `bonsai_zona` (una con `hasta`).
- [ ] Dar de baja un bonsai (fecha y tipo) y reactivarlo.
- [ ] Editar un bonsai y comprobar que se guardan los cambios.

## Decisiones tomadas
- Un solo archivo HTML con JavaScript puro, sin compilar; `supabase-js` desde CDN.
- Solo URL y clave publishable en el código.
- Crear especies desde el propio formulario, porque `especie_id` es obligatorio.
- Estilo con lista de sugerencias pero texto libre.
- No hay borrado de bonsais en la web (el borrado arrastraría eventos y fotos por `on delete cascade`). Para retirar un bonsai se usa la baja.
- Los campos `extra` (jsonb) no se tocan; quedan para ampliaciones.
- Taxonomía de `especies` corregida (`sql/02_especies_taxonomia.sql`): `epiteto_especifico` (antes `especie`), `especie` generada (género + epíteto), `autoria` nueva, `cultivar` (antes `variedad_cultivar`). `nombre_cientifico` = especie + cultivar entre comillas simples; si no hay cultivar, especie + autoría. El formulario crea especies con estos campos.
