# Contexto del Proyecto — Física para la Innovación Tecnológica

## Resumen rápido
Proyecto grupal para el curso de Física. Hay que exponer un tema y presentar
algo relacionado con la carrera (en este caso, **un programa / página web**).
Se eligió **un solo tema**.

El proyecto se centra **100% en simular el ciclo Otto de un motor de combustión
interna de 4 tiempos**, de forma dinámica e interactiva.

---

## Tema elegido
**Primera Ley de la Termodinámica** (tema central), con la **Segunda Ley
mencionada solo como límite** (por qué el motor nunca llega al 100%).

Idea central, una sola ecuación:

    ΔU = Q − W

- **ΔU** = cambio de energía interna del gas (se refleja en su temperatura)
- **Q**  = calor que se le mete al gas
- **W**  = trabajo que el gas realiza

En palabras: la energía no se crea ni se destruye; lo que entra (Q) menos lo
que sale como trabajo (W) queda como energía interna (ΔU). Es contabilidad de
energía.

---

## Cómo se plantean las dos leyes en el proyecto

### Primera Ley = el corazón del proyecto
El ciclo Otto se analiza contando energía en cada uno de sus 4 tiempos
(balance de Q, W y ΔU). Todo lo que muestra el programa —trabajo, calor que
entra, calor que sale, eficiencia— es Primera Ley aplicada al motor.

Los 4 tiempos del ciclo Otto (slides 25–26 del PDF de Primera Ley):
- **1 → 2  Compresión adiabática:** entra W (el pistón comprime), el gas se calienta. Q = 0.
- **2 → 3  Combustión isocórica:** entra Q (la chispa enciende la mezcla), sube mucho la T. W = 0.
- **3 → 4  Expansión adiabática:** el gas devuelve W (carrera de potencia). Q = 0.
- **4 → 1  Escape isocórico:** sale Q (se botan los gases). W = 0.
- Eficiencia teórica: η = 1 − r^(1−γ), donde r = relación de compresión.

### Segunda Ley = solo mención como límite
- Se menciona brevemente: la Segunda Ley explica POR QUÉ el motor nunca alcanza
  el 100% de eficiencia; siempre se desperdicia calor en el escape.
- NO se intenta demostrar visualmente con el motor (el motor no es buen ejemplo
  visual de la Segunda Ley; eso se mostraría mejor con entropía, mezclas, etc.).
- NO decir: "el ciclo Otto viene de la Segunda Ley".
- SÍ decir: "el ciclo Otto se analiza con la Primera Ley; la Segunda explica su
  límite de eficiencia".

---

## El programa

### Formato
- **Página web (HTML/JS)**, un solo archivo que se abre en el navegador.
- Razones: no instalar nada, a prueba de fallos el día de la exposición, se ve
  profesional, gráficas sin librerías pesadas.
- Estilo buscado: simulación física interactiva **tipo PhET** (objetos que se
  ven y reaccionan), NO un dashboard de tablas y números.
- Referencias dadas por el usuario:
  - PhET Faraday's Electromagnetic Lab (estilo de interacción).
  - GIF de motor 4 tiempos de Wikipedia (Ciclo Otto) como modelo visual del motor:
    https://es.wikipedia.org/wiki/Ciclo_Otto#/media/Archivo:4StrokeEngine_Ortho_3D_Small.gif

### Enfoque: UNA sola pantalla centrada en el motor Otto
(Se descartó la idea anterior de dos pestañas y se eliminó por completo el modo
"quemador / calentar-enfriar". Todo es el motor.)

Características deseadas:
- **Motor de 4 tiempos animado** (pistón, válvulas, cilindro) como el GIF de Wikipedia.
- **Cigüeñal arrastrable con el mouse**, representado como un **círculo aparte**
  que el usuario gira; al girarlo, el motor avanza por sus 4 tiempos de forma
  dinámica (sincronizado con el giro).
- Visualización del **calor**: el gas cambia de color según la fase
  (ej. rojo en combustión, azul/frío en escape/admisión).
- El usuario **elige la cantidad de gas (moles)** que entra.
- Mostrar en vivo todos los datos del tema:
  - Trabajo realizado (W)
  - Calor que entra (Q_entra) y calor que sale (Q_sale)
  - Energía interna (ΔU)
  - Eficiencia
  - Temperatura y presión
- Idealmente una **mini gráfica P-V** que se dibuja mientras se gira la manivela,
  mostrando el ciclo cerrándose.

### Notas de complejidad
- La parte de mayor complejidad es el **cigüeñal arrastrable sincronizado con la
  animación del motor** (mover el círculo con el mouse → el pistón y las válvulas
  responden en tiempo real). Es lograble pero exigente.
- Plan sugerido de construcción por partes:
  1. El motor animado girando solo (pistón + válvulas + ciclo).
  2. El cigüeñal/círculo arrastrable con el mouse que controla el giro.
  3. Los datos en vivo (W, Q, ΔU, eficiencia, T, P) + selección de moles.
  4. La mini gráfica P-V dibujándose en sincronía.

---

## Estado actual
- Plan acordado y enfoque redefinido: **todo gira en torno al motor Otto**.
- **Aún no se ha construido la versión final.**
- Demos exploratorias previas (dashboard y laboratorio gas+pistón) quedaron
  descartadas como producto final, pero sirvieron para fijar la dirección
  (interactivo, visual, estilo PhET, físicamente correcto).
- Próximo paso pendiente: construir el motor Otto animado (empezar por el paso 1).

---

## Aclaración física a tener presente (no romper)
- **Comprimir el gas (meter trabajo) lo calienta** (compresión adiabática) — en la compresión (1→2) el gas se calienta al acercarse el pistón a
  la culata.
- En la expansión (3→4) el gas se enfría al alejarse el pistón mientras entrega
  trabajo.
- **Quién provoca la combustión (importante, no confundir):**
  - En el motor de gasolina (ciclo Otto), **la chispa de la bujía enciende la
    mezcla**. La compresión la calentó y la preparó, PERO la compresión por sí
    sola NO la hace explotar. Si explotara sola por compresión, eso es un defecto
    llamado autodetonación / cascabeleo / pistoneo, y daña el motor.
  - En el motor **diésel** sí ocurre lo contrario: no hay bujía; el aire se
    comprime tanto que se calienta lo suficiente para que el combustible se
    encienda solo al inyectarlo (ignición por compresión).
  - Para este proyecto (ciclo Otto = gasolina): la causa de la combustión es la
    CHISPA, no el calor de la compresión.
  - Confirmado por el PDF: slide 25 "Chispa enciende la mezcla"; slide 26
    "combustión isocórica... combustión instantánea de la mezcla aire-combustible".

---

## Datos del curso (de los PDFs)
- Curso: Física para la Innovación Tecnológica — Universidad Autónoma del Perú.
- Entregable del grupo: subir archivo digital al Campus Virtual, un solo envío
  por grupo, con nombres completos de los integrantes.
- Productos aceptados: informe técnico breve, lámina A3, PPT, prototipo
  conceptual o análisis comparativo (el programa encaja como prototipo).
- Datos de referencia del ciclo Otto (PDF):
  - Eficiencia: η = 1 − r^(1−γ)
  - Motores reales alcanzan 25–35% (limitados por fricción, transferencia de
    calor no deseada y combustión incompleta).
