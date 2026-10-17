# AgentHub — Especificación del Panel de Administración

## 1. Descripción del producto

AgentHub es una plataforma SaaS donde las empresas alquilan agentes de IA: asistentes preconfigurados que se equipan con skills (capacidades como navegar por la web, leer documentos o gestionar calendarios) y se despliegan para tareas de negocio específicas.

El usuario de este panel es el **administrador interno de AgentHub**. Desde aquí supervisa la salud del negocio (ingresos, descuentos, agentes activos y fallando) y gestiona usuarios, agentes, el catálogo de skills, las contrataciones y los errores de ejecución.

Este documento es un prototipo visual con **datos hardcodeados**. No hay backend, API ni envío de formularios. El objetivo es que un agente de código pueda construir el panel completo leyendo solo este archivo, sin hacer preguntas.

## 2. Stack tecnológico y restricciones

- HTML5 semántico en un único archivo `index.html`.
- Tailwind CSS cargado **solo vía CDN** (`<script src="https://cdn.tailwindcss.com"></script>`) con `tailwind.config = { darkMode: 'class' }`.
- JavaScript vanilla en un único bloque `<script>` al final del `<body>`.
- Prohibido: frameworks (React, Vue…), jQuery, herramientas de build, archivos CSS propios, atributos `style` en línea y cualquier llamada a API o backend.
- Etiquetas semánticas obligatorias: `nav`, `header`, `main`, `section`, `table`, `thead`, `tbody`.
- Todas las secciones viven dentro de `<main>` y se muestran de una en una alternando la clase `hidden`. La página nunca se recarga, por lo que el modo oscuro se conserva al navegar.
- Layout usable en escritorio y tablet (≥ 768 px).

## 3. Datos compartidos (fuente única de verdad)

Los mismos nombres deben aparecer idénticos en todas las secciones.

**Agentes**

| Agente | Propietario | Estado | Skills |
|---|---|---|---|
| Atlas | NovaTech | Activo | Navegación web, Lectura de documentos |
| Scribe | Finlytics | Activo | Lectura de documentos, Gestión de calendarios |
| Sentinel | ShopWave | Fallando | Navegación web, Análisis de datos |
| Orbit | MediBot | Inactivo | Gestión de calendarios, Análisis de datos, Navegación web |

**Skills y precios**

| Skill | Precio | Agentes que la tienen |
|---|---|---|
| Navegación web | $15/mes | 3 (Atlas, Sentinel, Orbit) |
| Lectura de documentos | $20/mes | 2 (Atlas, Scribe) |
| Gestión de calendarios | $10/mes | 2 (Scribe, Orbit) |
| Análisis de datos | $25/mes | 2 (Sentinel, Orbit) |

**Colores de badges de estado**

- Verde: Activo, Resuelto.
- Ámbar: En prueba, Advertencia.
- Rojo: Suspendido, Fallando, Crítico.
- Gris: Inactivo, Finalizado, Pendiente.
- Azul: Info.

## 4. Especificaciones por sección

### 4.1 Layout global

1. **Sidebar persistente.** Un `<nav id="sidebar">` a la izquierda, siempre visible, ancho 14rem (`w-56`) en tablet y 16rem (`lg:w-64`) en escritorio, con fondo blanco (`dark:bg-gray-800`) y borde derecho. Arriba muestra el título "AgentHub" en negrita.
2. **Enlaces de navegación.** Seis enlaces en una lista vertical, en este orden: Dashboard, Usuarios, Agentes, Skills, Contrataciones, Log de errores. Cada enlace es un `<a href="#" data-nav="id-de-seccion">` en bloque, con padding y bordes redondeados.
3. **Indicador de sección activa.** El enlace de la sección visible tiene fondo azul (`bg-blue-600`) y texto blanco. Los demás tienen hover gris. Al hacer clic en un enlace se llama a `preventDefault()`, se ocultan todas las `<section>` de `<main>`, se muestra la elegida y se mueve el indicador. Al cargar, la activa es Dashboard.
4. **Header superior.** Un `<header>` con el título de la sección actual (`<h2 id="page-title">`) a la izquierda, que se actualiza con cada navegación, y el toggle de tema a la derecha.
5. **Toggle de modo oscuro.** Un botón `#theme-toggle` con el texto "🌙 / ☀️" que añade o quita la clase `dark` en `<html>`. Todos los componentes definen su variante `dark:` (fondos `gray-800/900`, texto claro, bordes `gray-700`). El modo elegido se mantiene al cambiar de sección.

### 4.2 Dashboard

1. **Cuadrícula de métricas.** Cuatro tarjetas en una cuadrícula responsive: 1 columna en móvil y 2×2 desde `sm:`, con separación `gap-4`. Cada tarjeta contiene un icono emoji a la izquierda, una etiqueta pequeña en gris y un valor grande en negrita.
2. **Contenido de las tarjetas.** (a) 💰 "Ingresos totales (mes)": $48,250. (b) 🏷️ "Pérdidas por descuentos y cupones": $3,120. (c) 🤖 "Agentes activos": 128. (d) ⚠️ "Agentes fallando": 7.
3. **Estilo de las tarjetas.** Fondo blanco (`dark:bg-gray-800`), esquinas `rounded-xl`, sombra sutil (`shadow-sm`) y un borde izquierdo grueso (`border-l-4`) con un color de acento distinto por métrica: verde, ámbar, azul y rojo respectivamente.
4. **Marcador del gráfico.** Debajo de las tarjetas, un `div` de ancho completo y 16rem de alto con borde discontinuo (`border-2 border-dashed`), esquinas redondeadas y la etiqueta "Gráfico de actividad semanal" centrada vertical y horizontalmente en gris.

### 4.3 Gestión de usuarios

1. **Tabla.** Una `<table>` dentro de una tarjeta redondeada con las columnas Nombre, Email, Plan, Estado y Acciones. Las filas se separan con un borde superior fino.
2. **Datos (5 filas).** Laura Gómez (laura@novatech.io, Pro, Activo), Carlos Ruiz (carlos@finlytics.com, Enterprise, Activo), María Torres (maria@shopwave.co, Starter, En prueba), Andrés Silva (andres@logix.dev, Pro, Suspendido), Sofía Herrera (sofia@medibot.health, Enterprise, Activo).
3. **Badge de estado.** Cada estado se muestra como una píldora (`rounded-full text-xs`) con color: verde para Activo, ámbar para En prueba, rojo para Suspendido.
4. **Dropdown de acciones.** La última celda de cada fila contiene un botón ⋮ que abre un menú con "Ver detalle" y "Eliminar" (en rojo). El menú se posiciona bajo el botón, alineado a la derecha, con sombra y `z-20`.
5. **Modal de detalle.** "Ver detalle" abre un modal con el registro completo: nombre, email, empresa, plan, estado, fecha de registro y número de agentes contratados. El modal se cierra con el botón ✕, con el botón "Cerrar" y al hacer clic en el backdrop.
6. **Eliminar.** "Eliminar" quita la fila del DOM.

### 4.4 Gestión de agentes

1. **Listado.** Cuatro tarjetas apiladas (`space-y-3`), una por agente. Cada tarjeta muestra el icono 🤖, el nombre en negrita, "Propietario: X", un badge de estado y el dropdown de acciones.
2. **Skills colapsables.** Bajo cada tarjeta hay un control "▸ Ver skills (N)" que revela la lista de skills como chips. Las skills están **ocultas por defecto**. Al hacer clic, el control cambia a "▾", la lista se despliega con una transición de 300 ms y un segundo clic la colapsa. Se implementa con `grid grid-rows-[0fr]` ↔ `grid-rows-[1fr]` y `transition-all duration-300`, sin `style` en línea.
3. **Dropdown de acciones.** El menú tiene "Configurar" y "Eliminar".
4. **Modal de configuración.** "Configurar" abre un modal titulado "Configurar agente: {nombre}" con un `<textarea>` editable (6 filas) que contiene el prompt de sistema de ese agente. Tiene los botones "Guardar" y "Cancelar", además de ✕ y cierre por backdrop. "Guardar" conserva el texto editado en la fila.
5. **Prompts.** Cada agente tiene un prompt propio de 1–2 frases que describe su rol (por ejemplo, Atlas: investigación web con citas de fuentes; Orbit: atención a pacientes sin diagnósticos médicos).

### 4.5 Skills

1. **Explicación.** En la parte superior, un recuadro azul claro con el título "¿Qué es una skill en AgentHub?" y un párrafo que explica que una skill es una capacidad que se adjunta a un agente, que cada agente se configura con las skills que necesita y que el cliente paga por las contratadas.
2. **Catálogo.** Cuatro tarjetas en una cuadrícula (1 columna en móvil, 2 desde `md:`): Navegación web, Lectura de documentos, Gestión de calendarios y Análisis de datos. Cada una muestra icono, nombre, descripción breve y un badge azul "N agentes la tienen habilitada" con los conteos de la sección 3.
3. **Dropdown.** Cada tarjeta tiene un menú ⋮ con "Ver detalle" y "Eliminar".
4. **Modal de detalle.** Muestra nombre, descripción completa, número de agentes que la usan y precio mensual.

### 4.6 Contrataciones

1. **Tabla.** Columnas: Cliente, Agente, Skills, Inicio – Fin, Importe pagado y Acciones. Cuatro contratos, activos y pasados.
2. **Datos.** NovaTech–Atlas (01/07/2026–31/12/2026, 6 meses, $210), Finlytics–Scribe (01/01/2026–31/12/2026, 12 meses, $360), ShopWave–Sentinel (01/08/2026–31/10/2026, 3 meses, $120), MediBot–Orbit (01/03/2026–30/06/2026, 4 meses, $200, finalizado). El importe es la suma de los precios de las skills del agente multiplicada por los meses.
3. **Dropdown.** Cada fila tiene un menú ⋮ con "Ver detalle" y "Eliminar".
4. **Modal de desglose.** "Ver detalle" muestra cliente, agente alquilado, lista de skills contratadas con su precio individual (una por línea, por ejemplo "Navegación web — $15/mes"), fechas de inicio y fin, duración, costo mensual, total pagado y estado.

### 4.7 Log de errores

1. **Tabla.** Columnas: Fecha y hora, Agente, Tipo, Descripción, Estado y Acciones. Seis entradas hardcodeadas con los agentes Sentinel (2), Atlas (2), Scribe (1) y Orbit (1).
2. **Badges por gravedad.** El tipo se muestra con un badge de color: rojo para Crítico, ámbar para Advertencia y azul para Info.
3. **Dropdown.** Cada fila tiene un menú ⋮ con "Ver detalle" y "Marcar como resuelto".
4. **Modal de traza.** "Ver detalle" muestra fecha y hora, agente, tipo, estado, descripción y la traza completa en un bloque monoespaciado con fondo gris y saltos de línea preservados.
5. **Marcar como resuelto.** Cambia el badge de estado de "Pendiente" (gris) a "Resuelto" (verde) y atenúa la fila con opacidad reducida.

### 4.8 Interacciones globales

1. **Cierre de dropdowns.** Un único listener de clic en `document` cierra todos los menús abiertos al hacer clic fuera de su área. Abrir un menú cierra los demás.
2. **Cierre de modales.** Todos los modales usan un contenedor `fixed inset-0 bg-black/50` que actúa como backdrop. Se cierran con el botón ✕, con cualquier botón `data-close` y al hacer clic en el backdrop. La tecla Escape también los cierra.
3. **Carga de datos en modales.** Las filas llevan los datos en atributos `data-*`. Al abrir un modal, un único listener genérico copia esos atributos a los elementos `[data-field]` del modal. Los `<textarea>` usan `.value` y el resto `.textContent`.

## 5. Inventario de componentes

| Componente | Dónde aparece | Descripción |
|---|---|---|
| Sidebar | Global | Navegación lateral fija con 6 enlaces e indicador de sección activa |
| Header | Global | Título de la sección y toggle de tema |
| Toggle de tema | Header | Alterna la clase `dark` en `<html>` |
| Tarjeta de métrica | Dashboard | Icono, etiqueta, valor y borde de acento |
| Marcador de gráfico | Dashboard | Área de ancho completo con borde discontinuo |
| Tabla | Usuarios, Contrataciones, Errores | Tabla en tarjeta con cabecera y filas separadas |
| Tarjeta de agente / skill | Agentes, Skills | Tarjeta con datos, badge y dropdown |
| Badge | Todas las secciones | Píldora de color para estado o gravedad |
| Dropdown de acciones | Todas las filas | Botón ⋮ con menú; cierra al hacer clic fuera |
| Modal | Todas las secciones | Overlay con backdrop, botón de cierre y campos `[data-field]` |
| Lista de skills colapsable | Agentes | Panel con transición de altura vía grid |
| Recuadro informativo | Skills | Explicación de qué es una skill |

## 6. Criterios de aceptación

1. `SPECS.md` está commiteado antes que cualquier archivo HTML del proyecto.
2. Las seis secciones existen y son accesibles desde el sidebar, y el enlace de la sección activa se resalta.
3. Al cargar, solo Dashboard es visible; el título del header cambia al navegar.
4. El Dashboard muestra cuatro tarjetas con icono, etiqueta y valor, en cuadrícula 2×2 en tablet y escritorio, y un área de borde discontinuo debajo.
5. Usuarios muestra al menos 5 filas con nombre, email, plan y badge de estado.
6. Agentes muestra al menos 4 agentes con nombre, propietario, badge de estado y lista de skills.
7. Las skills de cada agente están colapsadas por defecto; el clic las expande con una transición visible y el siguiente clic las colapsa.
8. Skills muestra al menos 4 skills con nombre, descripción y número de agentes, más una explicación de qué es una skill.
9. Contrataciones muestra al menos 4 contratos con cliente, agente, skills, fechas e importe pagado.
10. Log de errores muestra al menos 6 entradas con fecha y hora, agente, badge de color y descripción.
11. Todas las filas o tarjetas de los listados tienen un dropdown ⋮ que abre al hacer clic y se cierra al hacer clic fuera.
12. "Ver detalle" abre un modal en al menos cuatro secciones (Usuarios, Skills, Contrataciones, Errores).
13. "Configurar" en Agentes abre un modal con un `<textarea>` editable con el prompt del agente.
14. Todos los modales se cierran con el botón de cierre y con clic en el backdrop.
15. "Eliminar" quita el elemento y "Marcar como resuelto" actualiza el estado del error.
16. El toggle claro/oscuro cambia todo el panel mediante utilidades `dark:` y el modo se mantiene al cambiar de sección.
17. Los nombres de agentes (Atlas, Scribe, Sentinel, Orbit) y sus propietarios son idénticos en Agentes, Contrataciones y Log de errores.
18. El código no contiene atributos `style`, archivos CSS externos ni frameworks JS.
19. El HTML usa correctamente `section`, `table`, `nav`, `header` y `main`.
20. El layout es usable en viewports de escritorio y tablet.