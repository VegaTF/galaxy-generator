# Galaxy Generator

Generador de galaxias en 3D con partículas, construido con [Three.js](https://threejs.org/) y [Vite](https://vitejs.dev/). Ajusta los parámetros en tiempo real desde el panel de controles y observa cómo cambia la galaxia.

**Demo:** [galaxy-generator-gamma-topaz.vercel.app](https://galaxy-generator-gamma-topaz.vercel.app/)

## Características

- Hasta 100 000 partículas renderizadas con `THREE.Points` y `BufferGeometry`.
- Galaxia en espiral con brazos configurables.
- Gradiente de color entre el centro y el borde, con colores personalizables.
- Dispersión aleatoria controlable para dar un aspecto más natural a las estrellas.
- Panel de control ([lil-gui](https://lil-gui.georgealways.com/)) para regenerar la galaxia al instante.
- Cámara que orbita automáticamente alrededor de la galaxia.

## Controles

El panel de parámetros se muestra en la esquina superior derecha. Cada cambio regenera la galaxia al terminar de editar el valor.

| Parámetro | Valor por defecto | Rango | Descripción |
| --- | --- | --- | --- |
| `count` | `30000` | 100 – 100 000 | Número de partículas. |
| `size` | `0.01` | 0.001 – 0.1 | Tamaño de cada partícula. |
| `radius` | `3` | 0.01 – 20 | Radio de la galaxia. |
| `branches` | `5` | 2 – 20 | Número de brazos en espiral. |
| `spin` | `1` | -5 – 5 | Cuánto se curvan los brazos. Valores negativos invierten el giro. |
| `randomness` | `0.5` | 0 – 2 | Aleatoriedad de la posición. *Actualmente no se aplica en la generación.* |
| `randomnessPower` | `3` | 1 – 10 | Concentra la dispersión cerca del plano de la galaxia cuanto mayor es el valor. |
| `insideColor` | `#ff6030` | — | Color del centro. |
| `outsideColor` | `#1b3984` | — | Color de los bordes. |

## Requisitos

- [Node.js](https://nodejs.org/en/download/) (versión LTS recomendada)
- npm (incluido con Node.js)

## Instalación y uso

```bash
# Instalar dependencias (solo la primera vez)
npm install

# Arrancar el servidor de desarrollo (Vite muestra la URL, normalmente http://localhost:5173)
npm run dev

# Generar la versión de producción en la carpeta dist/
npm run build

# Desplegar en Vercel (requiere la CLI de Vercel configurada)
npm run deploy
```

## Estructura del proyecto

```
18-galaxy-generator/
├── src/
│   ├── index.html    # Página principal con el canvas WebGL
│   ├── script.js     # Escena, generación de la galaxia, cámara y render loop
│   └── style.css     # Estilos básicos (canvas a pantalla completa)
├── static/           # Recursos estáticos servidos tal cual
├── vite.config.js    # Configuración de Vite (raíz en src/, salida en dist/)
└── package.json
```

## Cómo funciona

1. **Posiciones.** Cada partícula recibe un radio aleatorio dentro de `radius` y se coloca en un brazo según `branches`. El ángulo se desplaza en función del radio (`spin`), lo que produce la espiral. Después se añade una pequeña dispersión en X, Y y Z.
2. **Colores.** El color de cada partícula es una interpolación lineal entre `insideColor` (centro) y `outsideColor` (borde), según su distancia al centro.
3. **Material.** Se usa `PointsMaterial` con mezcla aditiva (`AdditiveBlending`) y `vertexColors`, de forma que las zonas con más partículas brillan más.
4. **Regeneración.** Al cambiar un parámetro se liberan la geometría y el material anteriores (`dispose`) y se crea una nueva galaxia.

## Despliegue

El proyecto está desplegado en Vercel. Para publicar una nueva versión:

```bash
npm run deploy
```

## Créditos

Proyecto basado en la lección de galaxias del curso [Three.js Journey](https://threejs-journey.com/) de Bruno Simon.
