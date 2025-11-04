+++
date = '2025-11-03T19:59:03-05:00'
draft = true
title = 'Patrones'
+++

## Cómo un patrón de diseño transforma tu código en arte: del concepto a la visualización

### Introducción

En el corazón de la programación elegante reside una buena arquitectura. Los patrones de diseño — esas soluciones probadas para problemas recurrentes — no solo ayudan a organizar código, sino que pueden **convertir código funcional en piezas casi artísticas**. En este artículo exploraremos cómo el patrón Strategy Pattern puede ser ese hilo que te lleva del algoritmo al arte, mostrando cómo pasar de la teoría al código limpio y de allí a una visualización creativa que hace tangible el diseño.

Aquí no solo veremos *qué es* el patrón, sino *cómo aplicarlo* en código, y luego cómo transformarlo en una visualización que inspire, eduque y deleite.

---

### 1. ¿Qué es el Strategy Pattern?

El patrón Strategy pertenece a la categoría de patrones de comportamiento. Su objetivo: definir una familia de algoritmos, encapsular cada uno en su propia clase (o módulo) y hacerlos intercambiables en tiempo de ejecución. ([GeeksforGeeks][1])

En otras palabras: en lugar de tener un bloque `if/else` gigante que elija qué hacer en función de las condiciones, separamos cada comportamiento en su propia clase, y dejamos que el «cliente» o «contexto» simplemente escoja el que desee usar.

**Participantes típicos**:

* **Strategy** (interfaz o clase abstracta) — define el contrato para las estrategias.
* **ConcreteStrategy** — implementaciones concretas de esa interfaz.
* **Context** — contiene una referencia a una Strategy, y delega la operación a la Strategy actual. ([Visual Paradigm Tutorials][2])

**Ventajas clave**:

* Código más flexible y abierto a cambios (principio Open/Closed).
* Menos acoplamiento: el contexto no depende de detalles concretos.
* Nuevas estrategias pueden ser añadidas sin modificar el contexto. ([alesr][3])

---

### 2. Diseño del ejemplo: del patrón a la visualización

Para llevarlo de la teoría al arte vamos a plantear un ejemplo concreto que permita **visualizar** el patrón en acción.

**Contexto del ejemplo**: Imagina una aplicación web de visualización de partículas (o gráficos generativos) que puede emplear distintos comportamientos para mover o transformar las partículas: “movimiento lineal”, “movimiento radial”, “ondas sinusoides”, etc. Cada tipo de movimiento será una *estrategia*. El contexto será el motor de animación que recibe una estrategia y la ejecuta. Luego visualizaremos el resultado en un canvas.

#### 2.1 Arquitectura de clases

```text
MovimientoStrategy (interfaz)
│
├─ LinearMovementStrategy
├─ RadialMovementStrategy
└─ SineWaveMovementStrategy

AnimationContext
```

* `MovimientoStrategy`: define el método `update(particle, deltaTime)`.
* `LinearMovementStrategy`: implementa movimiento en línea recta.
* `RadialMovementStrategy`: movimiento radial hacia/desde el centro.
* `SineWaveMovementStrategy`: movimiento oscilatorio tipo onda seno.
* `AnimationContext`: recibe una estrategia, la aplica a cada partícula en cada frame.

#### 2.2 Código de ejemplo (JavaScript + Canvas)

```js
// Strategy interface
class MovementStrategy {
  update(particle, deltaTime) {
    throw new Error("update() must be implemented.");
  }
}

// Concrete Strategies
class LinearMovementStrategy extends MovementStrategy {
  update(p, dt) {
    p.x += p.vx * dt;
    p.y += p.vy * dt;
  }
}

class RadialMovementStrategy extends MovementStrategy {
  constructor(cx, cy) {
    super();
    this.cx = cx; this.cy = cy;
  }
  update(p, dt) {
    const dx = p.x - this.cx;
    const dy = p.y - this.cy;
    const angle = Math.atan2(dy, dx) + p.vr * dt;
    const dist = Math.hypot(dx, dy);
    p.x = this.cx + Math.cos(angle) * dist;
    p.y = this.cy + Math.sin(angle) * dist;
  }
}

class SineWaveMovementStrategy extends MovementStrategy {
  update(p, dt) {
    p.x += p.vx * dt;
    p.y = p.baseY + Math.sin(p.x * p.freq + p.phase) * p.amp;
  }
}

// Context
class AnimationContext {
  constructor(strategy) {
    this.strategy = strategy;
    this.particles = [];
  }
  setStrategy(strategy) {
    this.strategy = strategy;
  }
  addParticle(p) {
    this.particles.push(p);
  }
  update(dt) {
    this.particles.forEach(p => {
      this.strategy.update(p, dt);
    });
  }
  render(ctx) {
    ctx.clearRect(0,0,ctx.canvas.width,ctx.canvas.height);
    this.particles.forEach(p => {
      ctx.beginPath();
      ctx.arc(p.x, p.y, p.radius, 0, Math.PI*2);
      ctx.fillStyle = p.color;
      ctx.fill();
    });
  }
}
```

Con esta base, puedes crear diferentes partículas y cambiar dinámicamente la estrategia de movimiento para ver distintas “pinturas cinéticas”.

#### 2.3 Visualización y arte

Una vez que el motor está en marcha, la belleza aparece cuando:

* Cambias estrategias en tiempo real (por ejemplo al hacer clic o presionar tecla).
* Combinas estrategias: puedes incluso mezclar movimientos, modular parámetros como amplitud, frecuencia, velocidad, color.
* Añades visualizaciones extra: trazos, gradientes, efectos de desenfoque, layering.
* Usas la estrategia como metáfora: cada «estrategia» es un pincel o estilo de movimiento.

La clave aquí es que el patrón Strategy no solo organiza el código, sino que *da forma al comportamiento visual*. Cada estrategia es una forma artística distinta. Así el código no es solo funcional, sino creativo.

![Image](https://www.researchgate.net/publication/349931934/figure/fig2/AS%3A1013777336119296%401618714760094/Strategy-pattern-class-diagram.jpg)

![Image](https://i.pinimg.com/originals/5c/d7/ef/5cd7ef314fcbbf676788b6f5360823bb.png)

![Image](https://camo.githubusercontent.com/035537a7c3c02ee274f0d5e57aa634687ef6f43284212b0f5da1bbbd7e678b8e/68747470733a2f2f63646e2e7261776769742e636f6d2f6f747469732f63616e7661732d7061727469636c65732f6d61737465722f73637265656e732f73637265656e2e6a7067)

![Image](https://cruip.com/wp-content/uploads/2023/05/particle-animation-html-canvas-article-768x432.png)

![Image](https://i3.wp.com/onaircode.com/wp-content/uploads/2019/07/flowfield.png)

![Image](https://www.frontiersin.org/files/Articles/793914/fbinf-02-793914-HTML/image_m/fbinf-02-793914-g001.jpg)

---

### 3. Reflexión: por qué esto es arte

* **Modularidad = variabilidad**: Al tener estrategias independientes, puedes intercambiarlas, combinarlas, experimentar. Esto abre la puerta a “variaciones” artísticas sin reescribir todo el motor.
* **Comportamientos visuales como algoritmos**: Un algoritmo de movimiento no es solo matemática fría: puede ser un trazo, un ritmo, una danza de partículas.
* **Código claro = estética del software**: Cuando tu código sigue buenos patrones, no solo es mantenible, también ‘se ve’ bien — lo que corresponde a una estética del software.
* **Interacción y transformación**: Cambiar la estrategia es como cambiar el pincel o la paleta: permite al lector/usuario ver la metamorfosis del comportamiento.

---

### 4. Buenas prácticas y retos

* Asegúrate de mantener **cohesión**: cada estrategia debe tener una única responsabilidad.
* Evita que el contexto sepa detalles de cada estrategia — la interfaz debe abstraer lo suficiente.
* Piensa en rendimiento: los sistemas de visualización pueden volverse intensivos. Las estrategias deben estar optimizadas (evita lógica pesada en cada frame si no es necesario).
* Considera permitir **composición de estrategias**: por ejemplo, una estrategia de “movimiento” puede combinarse con una estrategia de “color” o “tamaño”.
* Documenta bien: ya que el arte puede implicar parámetros visuales, es útil que cada estrategia tenga metadatos claros (¿qué hace? ¿qué parámetros acepta?).

---

### 5. Conclusión

El patrón Strategy es mucho más que un recurso para organizar código: es una paleta de posibilidades para que el programa **se comporte** de distintas maneras, y si lo unes con visualización, se convierte en **una obra de arte interactiva**. Al separar el comportamiento (estrategias) del mecanismo (contexto) y luego aplicar esos comportamientos en una capa visual, fusionas arquitectura y estética.

Te invito a crear tu propio “lienzo de código”: selecciona una o varias estrategias, añade tus parámetros, observa el resultado, comparte tu visualización. Que tu lector vea no solo el *qué* del patrón, sino el *cómo* y el *por qué*. Aquí, el código se vuelve pincel, el patrón apuesta, y la visualización… arte.

---

¿Te gustaría que prepare un archivo de *plantilla de código descargable* (por ejemplo en GitHub Gist) con este ejemplo para que puedas integrarlo en tu blog? Puedo armarlo en JavaScript o en otro lenguaje que prefieras.

[1]: https://www.geeksforgeeks.org/strategy-pattern-set-1/?utm_source=chatgpt.com "Strategy Design Pattern - GeeksforGeeks"
[2]: https://tutorials.visual-paradigm.com/strategy-pattern-tutorial/?utm_source=chatgpt.com "Strategy Pattern Tutorial - Visual Paradigm Tutorials"
[3]: https://alesr.github.io/posts/strategy-pattern/?utm_source=chatgpt.com "Strategy Design Pattern | alesr"
