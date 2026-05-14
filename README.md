# Wanbe - Guía Interactiva de Trámites

**Wanbe** es una aplicación educativa creada en Python para guiar a los usuarios a través de trámites complejos del SAT (Sistema de Administración Tributaria) y RENAP (Registro Nacional de Personas). La aplicación proporciona información estructurada, requisitos y pasos detallados de forma interactiva.

## Características principales

- **Buscador recursivo** en la pantalla de inicio para encontrar trámites por nombre o palabra clave.
- **Navegación por categorías** del SAT y RENAP.
- **Checklist interactivo** de requisitos antes de iniciar cada trámite.
- **Guía paso a paso** con barra de progreso visual.
- **Persistencia de estado** para guardar el progreso del usuario entre sesiones.
- **Enlaces a portales oficiales** para acceder directamente a los servicios (si se abre en un navegador).

### Trámites disponibles

La aplicación actualmente incluye tres guías principales:

- **NIT** - Número de Identificación Tributaria
- **DPI** - Documento Personal de Identificación
- **Calcomanía** - Autorización de circulación vehicular

## Cómo funciona

### Flujo del usuario

1. **Inicio**: El usuario abre la aplicación y ve la pantalla de búsqueda.
2. **Búsqueda**: Utiliza el buscador recursivo para encontrar un trámite específico.
3. **Selección**: Elige una categoría y el trámite deseado.
4. **Requisitos**: Revisa el checklist de requisitos necesarios.
5. **Guía**: Sigue los pasos detallados para completar el trámite.
6. **Progreso**: La aplicación guarda su progreso automáticamente.

### Arquitectura técnica

La aplicación utiliza **estructuras de datos centralizadas** para organizar toda la información de trámites:

- **Diccionarios** (`TRAMITES`, `MENU`) contienen la información estructurada de cada trámite, sus requisitos y pasos.
- **Búsqueda recursiva** explora jerarquías de categorías para localizar trámites.
- **JSON** para persistencia de estado, guardando el progreso, checklist completado e historial de navegación.
- **Modulación clara** del código en capas independientes.

### Dos formas de uso

Wanbe se puede usar de dos maneras:

**Opción 1: Aplicación de escritorio**
- Interfaz gráfica con `tkinter`
- Experiencia nativa del sistema operativo
- Control total del estado local

**Opción 2: Vista web local**
- Servidor HTTP integrado
- Accesible desde cualquier navegador
- Interfaz moderna y responsiva

## Instalación y ejecución

### Requisitos previos

- Python 3.7 o superior
- `tkinter` (incluido por defecto en Python)

### Dependencias opcionales

```bash
python3 -m pip install -r requirements.txt
```

(Solo necesarias si deseas funcionalidades avanzadas como extracción de PDFs)

### Ejecutar la aplicación

**Opción 1: Aplicación de escritorio**

```bash
python3 proyecto.py
```

**Opción 2: Vista web local**

```bash
python3 vista_web.py
```

Luego abre en tu navegador:

```
http://127.0.0.1:8000
```

**Nota:** Si la vista web no se abre automáticamente, puedes pedirle a GitHub Copilot que abra la URL en el editor usando el comando **"Abre la vista web"**. Esto es útil si tu navegador no responde automáticamente a ciertos sistemas operativos.

## Cómo está organizado el código

El proyecto está modularizado en cuatro componentes principales:

### `data.py` - Capa de datos

Contiene toda la información centralizada:

- **`TRAMITES`**: Diccionario con la estructura de cada trámite (requisitos, pasos, enlaces).
- **`MENU`**: Estructura jerárquica de categorías para navegación.
- **`cargar_estado()`** y **`guardar_estado()`**: Funciones para persistencia en JSON con validación y respaldo automático.

### `utils.py` - Funciones de utilidad

Herramientas reutilizables:

- **`normalizar()`**: Convierte texto eliminando acentos para búsquedas insensibles a mayúsculas/minúsculas.
- **`buscar_recursivo()`**: Busca en la estructura jerárquica de forma recursiva.
- **`encontrar_categoria()`**: Localiza categorías dentro del árbol de datos.

### `app.py` - Lógica de la interfaz gráfica

La clase principal `WanbeApp`:

- Gestiona la interfaz gráfica con `tkinter`.
- Controla el flujo de navegación (pantallas, historial, cambio de pasos).
- Maneja el estado de la sesión (checklist, progreso actual).
- Incluye manejo robusto de errores con validación y alertas al usuario.

### `proyecto.py` - Punto de entrada

Script simple que inicia la aplicación de escritorio.

### `vista_web.py` - Servidor HTTP

Servidor local que renderiza la interfaz como página web HTML:

- Rutas HTTP para cada pantalla de la aplicación.
- Renderizado dinámico de contenido desde `data.py`.
- Compatible con navegadores modernos.

## Características técnicas destacadas

### Persistencia de estado

La aplicación guarda automáticamente el progreso del usuario en `data/state.json`:

- Paso actual en cada guía
- Checklist de requisitos completados
- Historial de navegación
- Preferencias del usuario

Si el archivo de estado se corrompe, la aplicación detecta el error y crea un respaldo automático con timestamp. El usuario puede reiniciar el estado borrando `data/state.json` y reabriendo la aplicación.

### Manejo de errores robusto

- **Validación de entrada**: Todas las búsquedas y selecciones se validan antes de procesarse.
- **Try/except en métodos críticos**: Cada operación importante (cargar estado, guardar progreso, navegar) tiene manejo de excepciones.
- **Alertas al usuario**: Mensajes claros informan sobre errores o problemas de estado.
- **Respaldos automáticos**: Si el JSON se corrompe, se crea un respaldo antes de intentar recuperarse.

### Búsqueda recursiva

La función `buscar_recursivo()` explora la estructura jerárquica de categorías de forma recursiva para encontrar trámites que coincidan con la consulta del usuario. Esto permite búsquedas flexibles sin importar la profundidad de la categoría.

## Reiniciar el estado

Si deseas volver a comenzar desde cero:

```bash
rm data/state.json
```

Luego abre la aplicación nuevamente y se creará un estado nuevo y limpio.

