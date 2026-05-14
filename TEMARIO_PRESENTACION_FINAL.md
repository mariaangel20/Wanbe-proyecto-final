# Temario de presentacion final - Wanbe

Este documento sirve como guia para explicar el proyecto en la defensa final. Incluye donde esta cada fragmento de codigo, que hace y que preguntas pueden hacer en la exposicion.

## 1. Resumen rapido del proyecto

Wanbe es una aplicacion creada en Python para orientar al usuario en tramites de SAT y RENAP. El proyecto tiene dos formas de uso:

- Aplicacion de escritorio con Tkinter.
- Vista web local con servidor HTTP.

La informacion de los tramites vive en estructuras de datos centrales y la navegacion usa busqueda recursiva, historial de pantallas y persistencia en JSON.

## 2. Mapa general del codigo

- [proyecto.py](proyecto.py#L9): punto de entrada del programa.
- [data.py](data.py#L18): datos, colores, tramites, menu y persistencia.
- [utils.py](utils.py#L10): funciones de busqueda y normalizacion.
- [app.py](app.py#L13): interfaz de escritorio con Tkinter.
- [vista_web.py](vista_web.py#L24): version web local con HTTP.

## 3. Explicacion por fragmentos de codigo

### 3.1 `proyecto.py`

Referencia principal: [proyecto.py](proyecto.py#L9)

- [proyecto.py](proyecto.py#L9) importa `WanbeApp` desde `app.py`. Eso separa el arranque de la logica real.
- [proyecto.py](proyecto.py#L12) define `main()`, que crea la aplicacion y llama a `ejecutar()`.
- [proyecto.py](proyecto.py#L17) verifica si el archivo se ejecuta directamente y entonces arranca la app.

Que decir en la defensa:

- Este archivo es solo el lanzador.
- Sirve para mantener compatibilidad con el comando `python proyecto.py`.
- La logica importante esta en `app.py`, `data.py` y `utils.py`.

### 3.2 `data.py`

Referencia principal: [data.py](data.py#L18)

#### Colores y rutas

- [data.py](data.py#L18) define `COLORS`, un diccionario con la paleta visual de toda la app.
- [data.py](data.py#L32) define `RUTAS`, una matriz con los recorridos principales de navegacion.

Que hace:

- `COLORS` evita repetir valores de color en toda la interfaz.
- `RUTAS` representa caminos de navegacion de forma estructurada.

#### Tramites

- [data.py](data.py#L39) define `TRAMITES`, el diccionario mas importante del proyecto.
- [data.py](data.py#L39) guarda los tramites `calcomania`, `nit` y `dpi`.
- Cada tramite contiene `titulo`, `portal`, `icono`, `descripcion`, `requisitos` y `pasos`.
- [data.py](data.py#L46) inicia la lista de pasos del tramite de calcomania.
- [data.py](data.py#L71) inicia el tramite de NIT.
- [data.py](data.py#L106) inicia el tramite de DPI.

Que hace cada fragmento:

- `requisitos` usa listas para mostrar lo necesario antes de empezar.
- `pasos` usa listas de diccionarios para describir cada etapa del tramite.
- `enlace` permite abrir portales oficiales desde la guia.

#### Menu jerarquico

- [data.py](data.py#L154) define `MENU`, un arbol de categorias y subcategorias.
- [data.py](data.py#L156) crea la categoria SAT.
- [data.py](data.py#L178) crea la categoria RENAP.

Que hace:

- `MENU` permite navegar por categorias.
- Tambien sirve como base para la busqueda recursiva.
- Cada nodo puede tener `hijos` que son mas categorias o tramites.

#### Persistencia

- [data.py](data.py#L193) define `cargar_estado()`.
- [data.py](data.py#L236) define `guardar_estado()`.

Que hace:

- `cargar_estado()` lee el JSON de estado de la sesion.
- Si el archivo esta corrupto, crea respaldo y devuelve un estado vacio.
- `guardar_estado()` escribe de forma atomica para evitar archivos incompletos.

### 3.3 `utils.py`

Referencia principal: [utils.py](utils.py#L10)

- [utils.py](utils.py#L10) define `normalizar()`, que pasa el texto a minusculas y quita tildes.
- [utils.py](utils.py#L16) define `buscar_recursivo()`, que explora el arbol `MENU`.
- [utils.py](utils.py#L56) define `encontrar_categoria()`, que localiza una categoria por id.

Que hace cada funcion:

- `normalizar()` hace que una busqueda funcione aunque el usuario escriba sin tildes o con mayusculas.
- `buscar_recursivo()` recorre nodos y subnodos para encontrar coincidencias.
- `encontrar_categoria()` busca una categoria dentro de toda la estructura jerarquica.

### 3.4 `app.py`

Referencia principal: [app.py](app.py#L13)

- [app.py](app.py#L13) define la clase `WanbeApp`.
- [app.py](app.py#L14) inicia la ventana principal y configura tamaño, titulo y color base.
- [app.py](app.py#L21) crea el historial de navegacion.
- [app.py](app.py#L22) y [app.py](app.py#L23) guardan el progreso y checklist por tramite.
- [app.py](app.py#L31) carga el estado persistido.
- [app.py](app.py#L43) construye la interfaz base.
- [app.py](app.py#L50) registra el cierre para guardar el estado.

- [app.py](app.py#L53) construye el marco principal.
- [app.py](app.py#L61) crea el boton de volver.
- [app.py](app.py#L73) carga el logo.
- [app.py](app.py#L95) agrega ayuda.
- [app.py](app.py#L106) agrega configuracion.
- [app.py](app.py#L117) y [app.py](app.py#L120) crean el canvas y el scroll.

- [app.py](app.py#L152) define `cambiar_pantalla()`.
- [app.py](app.py#L163) define `volver()`.
- [app.py](app.py#L169) actualiza el texto del encabezado.

- [app.py](app.py#L263) define `mostrar_inicio()`.
- [app.py](app.py#L299) pinta el menu inicial.
- [app.py](app.py#L305) define la busqueda por texto.
- [app.py](app.py#L312) llama a `buscar_recursivo()`.

- [app.py](app.py#L355) define `mostrar_categoria()`.
- [app.py](app.py#L368) recorre los hijos de la categoria.
- [app.py](app.py#L376) define `mostrar_tramite()`.

- [app.py](app.py#L396) define `mostrar_checklist()`.
- [app.py](app.py#L451) define `iniciar_tramite()`.
- [app.py](app.py#L470) define `mostrar_paso()`.
- [app.py](app.py#L530) define `navegacion_pasos()`.
- [app.py](app.py#L589) define `finalizar()`.

- [app.py](app.py#L613) define `mostrar_ayuda()`.
- [app.py](app.py#L624) define `aplicar_configuracion()`.
- [app.py](app.py#L630) define `mostrar_configuracion()`.
- [app.py](app.py#L729) define `_mostrar_alerta_carga()`.
- [app.py](app.py#L739) define `_on_close()`.

### 3.5 `vista_web.py`

Referencia principal: [vista_web.py](vista_web.py#L24)

- [vista_web.py](vista_web.py#L24) define `BASE_DIR`.
- [vista_web.py](vista_web.py#L25) define `HOST`.
- [vista_web.py](vista_web.py#L26) define `DEFAULT_PORT`.
- [vista_web.py](vista_web.py#L32) define `crear_ruta()`.
- [vista_web.py](vista_web.py#L41) define `crear_ruta_tramite()`.
- [vista_web.py](vista_web.py#L49) define `crear_nav_activo()`.
- [vista_web.py](vista_web.py#L58) define `render_layout()`.
- [vista_web.py](vista_web.py#L440) define `render_inicio()`.
- [vista_web.py](vista_web.py#L501) define `render_categoria()`.
- [vista_web.py](vista_web.py#L553) define `render_tramite_resumen()`.
- [vista_web.py](vista_web.py#L653) define `render_tramite()`.
- [vista_web.py](vista_web.py#L767) define `WanbeHandler`.
- [vista_web.py](vista_web.py#L782) define `do_GET()`.
- [vista_web.py](vista_web.py#L835) define `send_html()`.
- [vista_web.py](vista_web.py#L851) define `send_asset()`.
- [vista_web.py](vista_web.py#L872) define `run_server()`.
