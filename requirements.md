# Requirements — Simulador del Ciclo Otto (Primera Ley de la Termodinámica)

> Documento de requisitos para construir el programa del proyecto de Física.
> Pensado para ser leído por Claude Code junto con `context.md`.
> Siguiente paso después de este archivo: generar un `solutions.md` (diseño técnico/implementación).

---

## 1. Objetivo del programa
Una página web interactiva que **simula el ciclo Otto** de un motor de combustión
interna de 4 tiempos, de forma visual y dinámica (estilo PhET), para **demostrar
la Primera Ley de la Termodinámica** (ΔU = Q − W) aplicada al motor.

La Segunda Ley solo se menciona como límite (por qué la eficiencia nunca llega
al 100%); NO se simula.

El detalle conceptual completo (física, los 4 tiempos, aclaraciones chispa vs
compresión, Otto vs diésel) está en `context.md` y no se repite aquí.

---

## 2. Stack técnico (decidido)
- **HTML + CSS + JavaScript puro (vanilla).**
- **Un solo archivo `.html`** con todo embebido (CSS y JS adentro).
- **Canvas 2D a mano** para TODO el dibujo: el motor y la gráfica P-V.
- **Sin librerías externas** (ni Chart.js ni frameworks). Nada que cargue desde
  internet.

### Restricciones prácticas (obligatorias)
- Debe **funcionar 100% offline**: abrir el `.html` con doble clic, sin conexión.
- **Sin instalar nada** en la máquina.
- Interfaz **en español**.
- Navegador objetivo: navegadores de escritorio modernos (Chrome/Edge/Firefox).
- No usar `localStorage`/`sessionStorage` ni APIs que requieran servidor.

---

## 3. Estética
- **Estilo oscuro tipo "laboratorio / simulador"**: fondo oscuro, el motor y los
  datos resaltan sobre él.
- Limpio y legible; prioridad a que se ENTIENDA el ciclo, no a la decoración.
- El gas debe cambiar de color según su estado térmico (frío → caliente),
  p. ej. azul (admisión/frío) → rojo intenso (combustión).

### Nivel visual de referencia (2D, NO 3D)
El objetivo es un **corte 2D con piezas móviles**, claro y bien acabado. NO se
busca realismo 3D (se descartó Three.js). Referencias acordadas con el usuario:
- **Corte de motor tipo esquema gris** con válvulas, biela, cigüeñal y flecha de
  color que indica el flujo del gas (entrada/salida). Acabado pulido: piezas con
  sombreado, gas coloreado. (Imagen 1 de referencia del usuario.)
- **Esquema simple etiquetado de motor 4 tiempos** (pistón, biela, cigüeñal,
  válvula de admisión/escape, aletas). Claridad ante todo. (Imagen 2.)
- **Meta:** algo intermedio — la claridad del esquema etiquetado + el acabado más
  pulido del corte gris, con todas las piezas animadas (pistón sube/baja, biela y
  cigüeñal giran, válvulas abren/cierran, chispa, gas que cambia de color).
- Si se puede mejorar/hacer más vistoso manteniéndolo 2D, mejor.

---

## 4. Alcance
**Versión completa de una sola vez** (no por fases). El equipo es numeroso y hay
~1 mes de preparación, así que se construye todo el conjunto de features descrito
abajo desde el inicio.

---

## 5. Funcionalidades

### 5.1 Motor animado (Canvas 2D)
- Vista de **corte 2D** del cilindro que muestra el interior (como el GIF de
  referencia de Wikipedia en `context.md`).
- Piezas visibles y animadas en sincronía: **pistón, biela, cigüeñal y válvulas**
  (admisión y escape abriendo/cerrando según el tiempo).
- El motor recorre los **4 tiempos** del ciclo Otto:
  1. Admisión
  2. Compresión (el gas se calienta al comprimirse — respetar física de `context.md`)
  3. Combustión + expansión / carrera de potencia (la chispa enciende la mezcla)
  4. Escape
- **Indicador visual de la chispa** en el momento de la combustión.
- El **color del gas** cambia con su temperatura/estado a lo largo del ciclo.

### 5.2 Cigüeñal arrastrable con el mouse
- Un **círculo/manivela aparte** (control dedicado) que el usuario **gira
  arrastrando con el mouse**.
- Al girarlo, el motor **avanza por sus 4 tiempos sincronizado** con el ángulo
  del cigüeñal (girar adelante/atrás mueve el ciclo adelante/atrás).
- Debe sentirse fluido y directo (el pistón y válvulas responden en tiempo real).

### 5.3 Controles del usuario (inputs)
- **Cantidad de gas (moles, n):** seleccionable.
- **Relación de compresión (r):** ajustable (afecta la eficiencia; muy didáctico).
- **Calor de combustión:** ajustable (cuánta energía aporta la "explosión" /
  cuánto sube la temperatura en la combustión).
- **Modo de giro automático:** botón para que el motor gire solo, **además** del
  arrastre manual, con **velocidad de giro ajustable**.

### 5.4 Datos mostrados (AMBOS formatos)
Mostrar todos estos valores: **W (trabajo), Q que entra, Q que sale, ΔU (energía
interna), eficiencia, temperatura y presión.**

- **(a) Números en vivo:** valores que cambian en tiempo real mientras el motor
  gira (en tarjetas o panel lateral).
- **(b) Tabla de los 4 estados:** valor de las variables en cada punto del ciclo
  (estado 1, 2, 3, 4).

### 5.5 Gráfica P-V (Canvas 2D a mano)
- Diagrama Presión–Volumen del ciclo Otto (figura cerrada de 4 tramos).
- **Sincronizada con el motor:** un punto/marcador recorre la curva P-V conforme
  gira el cigüeñal, mostrando en qué parte del ciclo está el motor.
- Dibujada a mano en Canvas (sin librerías), para que funcione offline y se
  sincronice con la animación.

---

## 6. Física y fórmulas que el programa debe calcular
> Base: ciclo Otto ideal con aire/gas como gas ideal. Ajustar constantes según
> se vea conveniente en `solutions.md`.

- Ecuación de estado del gas ideal: `P·V = n·R·T`  (R = 8.314 J·mol⁻¹·K⁻¹)
- **Eficiencia teórica:** `η = 1 − r^(1−γ)`  (γ = Cp/Cv; aire ≈ 1.4)
- **Compresión adiabática (1→2):** `T·V^(γ−1) = constante`  → `T2 = T1·r^(γ−1)`,
  con `Q = 0`, `W = −ΔU = −n·Cv·(T2−T1)`.
- **Combustión isocórica (2→3):** `V = constante`, `W = 0`,
  `Q_entra = n·Cv·(T3−T2)`.
- **Expansión adiabática (3→4):** `Q = 0`, `W = −ΔU = −n·Cv·(T4−T3)` (trabajo
  positivo del gas), `T4 = T3·(1/r)^(γ−1)`.
- **Escape isocórico (4→1):** `V = constante`, `W = 0`,
  `Q_sale = n·Cv·(T4−T1)`.
- **Trabajo neto del ciclo:** `W_neto = Q_entra − Q_sale`.
- **Verificación Primera Ley:** en el ciclo completo `ΔU_total = 0`, por lo que
  `W_neto = Q_entra − Q_sale` (debe cumplirse en los números mostrados).
- Cv y Cp para gas ideal: `Cp − Cv = R`, `γ = Cp/Cv`. Para aire diatómico:
  `Cv ≈ (5/2)R`, `Cp ≈ (7/2)R`, `γ ≈ 1.4` (definir como constantes claras).
- Los inputs del usuario (n, r, calor de combustión) alimentan estas fórmulas y
  recalculan datos + gráfica en vivo.

---

## 7. Lo que NO entra (fuera de alcance)
- No simular la Segunda Ley (solo mención textual del límite de eficiencia).
- No modo "quemador / calentar-enfriar" (descartado).
- No 3D / Three.js / WebGL (se eligió Canvas 2D).
- No créditos ni nombres del grupo en la página (van aparte en el PDF).
- No backend, base de datos, ni guardado de estado.

---

## 8. Criterios de "terminado"
- El motor se anima correctamente por los 4 tiempos y las piezas se mueven en
  sincronía coherente (pistón–biela–cigüeñal–válvulas).
- Girar el cigüeñal con el mouse mueve el ciclo en tiempo real, en ambos sentidos.
- El modo automático gira solo y su velocidad es ajustable.
- Cambiar n, r y el calor de combustión actualiza datos y gráfica al instante.
- Los números en vivo y la tabla de 4 estados son coherentes entre sí y cumplen
  la Primera Ley (W_neto = Q_entra − Q_sale; ΔU del ciclo = 0).
- La gráfica P-V se dibuja correctamente y el marcador sigue al motor.
- La física respeta `context.md` (compresión calienta; la chispa —no la
  compresión— enciende la mezcla en el Otto).
- Todo corre abriendo el `.html` sin internet ni instalación.
- Interfaz en español, estilo oscuro de laboratorio.

---

## 9. Siguiente paso
Con este `requirements.md` + `context.md`, generar un **`solutions.md`** que
defina: estructura del código, cómo modelar la geometría del motor
(pistón/biela/cigüeñal con trigonometría), el bucle de animación, el mapeo
ángulo-del-cigüeñal → estado del ciclo, y cómo se calcula y dibuja la P-V.
