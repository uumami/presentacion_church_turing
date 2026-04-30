---
theme: default
title: la tesis de church-turing
info: |
  computabilidad · mente · máquina
  mario vázquez corte. dacc itam.
colorSchema: dark
class: 'theme-lain'
fonts:
  sans: 'Inter'
  mono: 'IBM Plex Mono'
  serif: 'Cinzel'
transition: fade
mdc: true
routerMode: hash
drawings:
  enabled: true
  persist: false
canvasWidth: 1280
---

<div class="cover-wrap">
  <img src="/img/01_ct_cover.png" class="bg" onerror="this.style.display='none'" />
  <div class="grid-bg"></div>
  <MatrixRain />
  <div class="status-bar">
    <span class="dot-live"></span>
    <span>wired</span>
    <span class="sep">//</span>
    <span>computabilidad</span>
    <span class="sep">//</span>
    <span>mente</span>
  </div>
  <div class="cover-title">
    <h1>la tesis de<br>church-turing</h1>
    <div class="cover-sub"><Slashes :items="['computabilidad', 'mente', 'máquina']" /></div>
  </div>

  <div class="qr-cover">
    <QrCode
      value="https://uumami.wiki/presentacion_church_turing/"
      :size="160"
      background="#0b0f0d"
      foreground="#7fffb5"
    />
    <div class="qr-cover-label">
      <span class="arrow">↑</span>
      <span>síguela en tu teléfono</span>
    </div>
  </div>

  <div class="glyph tl">∀</div>
  <div class="glyph tr">δ</div>
  <div class="glyph bl">λ</div>
  <div class="scan"></div>
  <div class="vignette"></div>
</div>

<style>
.cover-wrap {
  position: absolute; inset: 0;
  background:
    radial-gradient(ellipse at 65% 35%, rgba(127,255,181,0.07), transparent 55%),
    radial-gradient(ellipse at 20% 75%, rgba(46,245,255,0.05), transparent 50%),
    #0b0f0d;
  overflow: hidden;
}
.cover-wrap .bg {
  position: absolute; inset: 0; width: 100%; height: 100%;
  object-fit: cover; opacity: 0.65; z-index: 1;
}
.cover-wrap .grid-bg {
  position: absolute; inset: 0; z-index: 2;
  background-image:
    linear-gradient(rgba(127,255,181,0.07) 1px, transparent 1px),
    linear-gradient(90deg, rgba(127,255,181,0.07) 1px, transparent 1px);
  background-size: 80px 80px;
  mask-image: radial-gradient(ellipse at 50% 60%, black 10%, transparent 75%);
  pointer-events: none;
}
.cover-title {
  position: absolute;
  left: 3.5rem; bottom: 4rem;
  z-index: 10;
}
.cover-title h1 {
  font-family: 'Inter', system-ui, sans-serif;
  font-weight: 300;
  font-size: 5rem;
  line-height: 1.05;
  color: #7fffb5;
  text-shadow: 0 0 30px rgba(127,255,181,0.4);
  letter-spacing: -0.02em;
  text-transform: lowercase;
  margin: 0 0 0.8rem 0;
}
.cover-sub {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 1rem;
  color: #e8fff3;
  opacity: 0.55;
  letter-spacing: 0.08em;
}
.status-bar {
  position: absolute;
  top: 2.5rem; left: 3.5rem;
  z-index: 10;
  display: flex; align-items: center; gap: 0.5rem;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 0.82rem;
  color: #2ef5ff;
  letter-spacing: 0.08em;
  opacity: 0.75;
}
.status-bar .dot-live {
  width: 7px; height: 7px; border-radius: 999px;
  background: #2ef5ff;
  box-shadow: 0 0 8px #2ef5ff;
  animation: pulse 1.4s ease-in-out infinite;
}
.status-bar .sep { opacity: 0.3; }
@keyframes pulse {
  0%, 100% { opacity: 1; transform: scale(1); }
  50% { opacity: 0.35; transform: scale(0.7); }
}
.qr-cover {
  position: absolute;
  right: 3.5rem; bottom: 3.5rem;
  z-index: 10;
  display: flex; flex-direction: column; align-items: center; gap: 0.5rem;
}
.qr-cover-label {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 0.72rem;
  color: #7fffb5;
  opacity: 0.55;
  display: flex; align-items: center; gap: 0.35rem;
}
.glyph {
  position: absolute;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 8rem;
  font-weight: 300;
  color: #7fffb5;
  opacity: 0.04;
  z-index: 3;
  user-select: none;
  line-height: 1;
}
.glyph.tl { top: 1.5rem; left: 2rem; }
.glyph.tr { top: 1.5rem; right: 2rem; }
.glyph.bl { bottom: 1.5rem; left: 2rem; font-size: 10rem; }
.scan {
  position: absolute; left: 0; right: 0;
  height: 2px;
  background: linear-gradient(90deg, transparent, rgba(127,255,181,0.6), transparent);
  animation: scanLine 6s linear infinite;
  z-index: 5; pointer-events: none;
}
@keyframes scanLine {
  0% { top: -2px; opacity: 0; }
  5% { opacity: 1; }
  95% { opacity: 0.6; }
  100% { top: 100%; opacity: 0; }
}
.vignette {
  position: absolute; inset: 0; z-index: 4; pointer-events: none;
  background: radial-gradient(ellipse at 50% 50%, transparent 50%, rgba(0,0,0,0.55) 100%);
}
</style>

---
layout: two-cols
class: theme-lain layer-cyberpunk
---

# alan turing
<div style="font-family:var(--font-mono);font-size:0.78rem;color:#2ef5ff;opacity:0.6;margin-bottom:0.8rem;letter-spacing:0.07em">1912 · 1954 · manchester</div>

<div class="kv" style="margin-top:0rem;font-size:1.05rem">
  <div><span class="k">1936</span> <span class="v">· <strong>"on computable numbers"</strong> — define el algoritmo, inventa la MT, prueba que hay problemas irresolubles</span></div>
  <div><span class="k">parada</span> <span class="v">· demostró que ningún algoritmo puede decidir si otro programa termina o corre infinito — el primer límite formal de la computación</span></div>
  <div><span class="k">1939–45</span> <span class="v">· diseñó la <strong>Bombe</strong> para descifrar Enigma. se estima que acortó la WWII 2–4 años y salvó millones de vidas</span></div>
  <div><span class="k">ACE</span> <span class="v">· propuso el diseño de una computadora de propósito general con programa almacenado — base de toda arquitectura moderna</span></div>
  <div><span class="k">1950</span> <span class="v">· <strong>"computing machinery and intelligence"</strong> — propone el test de turing: si no puedes distinguirla de una persona, ¿importa si "piensa"?</span></div>
  <div><span class="k">1952</span> <span class="v">· <strong>morfogénesis</strong> — modelo matemático de cómo surgen patrones en la naturaleza: manchas de leopardo, espirales de caracol. une biología y cómputo</span></div>
</div>

<p style="margin-top:1rem; opacity:0.5; font-size:1.05rem">
  fue perseguido y condenado por el estado británico por ser gay. murió a los 41 años. indultado póstumamente en 2013.
</p>

::right::

<img src="/img/02_turing.png" class="slide-img" onerror="this.style.display:'none'" style="height:90%;object-fit:cover;border-radius:8px;opacity:0.82" />

---
layout: two-cols
class: theme-frieren layer-vaporwave
---

# alonzo church

<div class="kv" style="margin-top:0.5rem">
  <div><span class="k">1903–1995</span> <span class="v">· washington, d.c.</span></div>
  <div><span class="k">trabajo</span> <span class="v">· lógico matemático, princeton</span></div>
  <div><span class="k">λ-cálculo</span> <span class="v">· sistema formal para computar funciones</span></div>
  <div><span class="k">1936</span> <span class="v">· mismo resultado que turing, independiente</span></div>
  <div><span class="k">resultado</span> <span class="v">· el λ-cálculo y la MT son equivalentes</span></div>
</div>

<p style="margin-top:1.5rem; opacity:0.65; font-size:1.2rem">
  dos personas, dos formalismos distintos, el mismo año.
  esa coincidencia es parte de la tesis.
</p>

::right::

<img src="/img/03_church.png" class="slide-img" onerror="this.style.display:'none'" style="height:80%;object-fit:cover;border-radius:8px;opacity:0.82" />

---
layout: default
class: theme-eva layer-cyberpunk
---

# la máquina de turing

<div style="display:grid;grid-template-columns:1fr 1.1fr;gap:2.5rem;margin-top:0.3rem">
<div>

<div class="formula" style="margin-bottom:1rem;font-size:1.6rem">
  M = (<span class="sym">Q</span>, <span class="sym">Γ</span>, <span class="sym">b</span>, <span class="sym">Σ</span>, <span class="sym">δ</span>, <span class="sym">q₀</span>, <span class="sym">F</span>)
</div>

<div class="kv" style="font-size:1.05rem">
  <div><span class="k">Q</span> <span class="v">· conjunto finito de <strong>estados</strong></span></div>
  <div><span class="k">Γ</span> <span class="v">· <strong>alfabeto</strong> de la cinta</span></div>
  <div><span class="k">b</span> <span class="v">· símbolo <strong>blanco</strong> — casilla vacía</span></div>
  <div><span class="k">Σ</span> <span class="v">· símbolos de <strong>entrada</strong> (sin el blanco)</span></div>
  <div><span class="k">δ</span> <span class="v">· función de <strong>transición</strong> — las instrucciones</span></div>
  <div><span class="k">q₀</span> <span class="v">· estado <strong>inicial</strong></span></div>
  <div><span class="k">F</span> <span class="v">· estados de <strong>aceptación</strong> — parada</span></div>
</div>

<div class="formula" style="font-size:1.1rem;margin-top:0.8rem;opacity:0.7">
  δ: Q × Γ → Q × Γ × {<span class="sym">L</span>, <span class="sym">R</span>}
</div>

</div>

<div style="display:flex;flex-direction:column;gap:1.1rem">

  <div>
    <div style="font-family:var(--font-mono);font-size:0.8rem;color:var(--eva-amber);letter-spacing:0.07em;margin-bottom:0.25rem">LA CINTA</div>
    <p style="font-size:1.15rem;opacity:0.78;line-height:1.5;margin:0">tira <strong>infinita</strong> de casillas. cada casilla tiene un símbolo, o está vacía (b = "_"). es la memoria ilimitada de la máquina.</p>
  </div>

  <div>
    <div style="font-family:var(--font-mono);font-size:0.8rem;color:var(--eva-amber);letter-spacing:0.07em;margin-bottom:0.25rem">LA CABEZA LECTORA</div>
    <p style="font-size:1.15rem;opacity:0.78;line-height:1.5;margin:0">apunta a una casilla. en cada paso: <strong>lee</strong> el símbolo, <strong>escribe</strong> uno (puede ser el mismo), y se mueve <strong>L</strong> (← izquierda) o <strong>R</strong> (→ derecha).</p>
  </div>

  <div>
    <div style="font-family:var(--font-mono);font-size:0.8rem;color:var(--eva-amber);letter-spacing:0.07em;margin-bottom:0.25rem">LOS ESTADOS (Q)</div>
    <p style="font-size:1.15rem;opacity:0.78;line-height:1.5;margin:0">la única "memoria interna" de la máquina. no acumula historia: solo importa <strong>en qué estado está ahora</strong>. como un semáforo — solo un color a la vez.</p>
  </div>

  <div>
    <div style="font-family:var(--font-mono);font-size:0.8rem;color:var(--eva-amber);letter-spacing:0.07em;margin-bottom:0.25rem">δ — EL PROGRAMA COMPLETO</div>
    <p style="font-size:1.15rem;opacity:0.78;line-height:1.5;margin:0">una tabla que dice: "si estoy en el estado X y leo Y → escribe Z, muévete en D, cambia al estado W." eso es <em>todo</em> el programa.</p>
  </div>

</div>
</div>

---
layout: default
class: theme-eva layer-cyberpunk
---

# la máquina concreta

<TuringRules />

---
layout: default
class: theme-lain
---

# paso a paso

<div style="margin-top:0.4rem">
  <TuringTrace />
</div>

---
layout: default
class: theme-lain
---

# pruébalo

<div style="margin-top:0.4rem">
  <TuringMachine />
</div>

---
layout: default
class: theme-eva
---

# la tesis church-turing

<div style="margin-top:2rem">
<div class="big-quote theme-eva-q" style="font-size:1.65rem;max-width:85%">
  toda función que es <em>efectivamente computable</em> puede ser computada
  por una Máquina de Turing.
</div>
</div>

<div style="margin-top:2.5rem;display:grid;grid-template-columns:1fr 1fr;gap:2rem">
<div>
<p style="opacity:0.75;font-size:1.3rem">
  "efectivamente computable" = existe un procedimiento mecánico,
  paso a paso, determinístico, que siempre termina.
</p>
</div>
<div>
<p style="opacity:0.75;font-size:1.3rem">
  no es un teorema demostrable. es una <em>tesis</em> — una hipótesis
  sobre la naturaleza del cómputo que nadie ha refutado en 90 años.
</p>
</div>
</div>

---
layout: default
class: theme-lain layer-vaporwave
---

# la extensión filosófica

<div style="display:grid;grid-template-columns:1.1fr 1fr;gap:3rem;margin-top:1rem;align-items:start">
<div>
<div class="chain">
  <div class="step highlight">función computable</div>
  <div class="arrow">↓  cualquier algoritmo</div>
  <div class="step">proceso efectivo paso a paso</div>
  <div class="arrow">↓  ¿todo proceso físico?</div>
  <div class="step">cualquier proceso mecánico</div>
  <div class="arrow">↓  ¿incluyendo el pensamiento?</div>
  <div class="step">procesos cognitivos</div>
  <div class="arrow">↓  ¿el cerebro es físico?</div>
  <div class="step highlight">¿somos Máquinas de Turing?</div>
</div>
</div>
<div>
<img src="/img/07_extension.png" style="width:100%;height:90%;object-fit:cover;border-radius:8px;opacity:0.78" onerror="this.style.display='none'" />
</div>
</div>

---
layout: default
class: theme-frieren
---

# ojo: la tesis no dice eso

<div style="margin-top:1.5rem">
<div class="warning">
<p>la tesis de church-turing habla de <strong>funciones matemáticas</strong>, no de mentes, cerebros ni consciencia.</p>
</div>
</div>

<div style="display:grid;grid-template-columns:1fr 1fr;gap:2.5rem;margin-top:1.5rem">
<div>
<p style="font-size:1.3rem;line-height:1.6">
  <strong style="color:var(--frieren-gold)">lo que dice:</strong><br>
  toda función computable tiene una MT equivalente.
</p>
<p style="font-size:1.3rem;line-height:1.6;margin-top:1rem">
  <strong style="color:var(--frieren-gold)">lo que no dice:</strong><br>
  que la mente humana sea un proceso computable.
</p>
</div>
<div>
<p style="font-size:1.3rem;line-height:1.6">
  el salto de "computable" a "pensable" es una extensión filosófica — válida como pregunta, pero no como conclusión de la tesis.
</p>
<p style="font-size:1.2rem;line-height:1.6;opacity:0.6;margin-top:1rem">
  conviene ser honestos sobre qué dice la tesis y qué nos invita a preguntarnos.
</p>
</div>
</div>

---
layout: default
class: theme-frieren layer-solarpunk
---

# argumentos a favor

<img src="/img/09_afavor.png" class="slide-img-right" onerror="this.style.display='none'" />

<div style="max-width:55%;margin-top:0.5rem">
<ul>
  <li><strong>materialismo:</strong> el cerebro es físico. lo físico es en principio simulable.</li>
  <li><strong>neurociencia:</strong> las neuronas disparan en pulsos discretos — spikes. parece computable.</li>
  <li><strong>Church-Turing-Deutsch:</strong> cualquier proceso físico puede ser simulado por una computadora universal.</li>
  <li><strong>el propio turing:</strong> "si una persona es un sistema físico finito, entonces su comportamiento es computable."</li>
</ul>
</div>

---
layout: default
class: theme-elden layer-cyberpunk
---

# argumentos en contra

<img src="/img/10_encontra.png" class="slide-img-right" onerror="this.style.display='none'" />

<div style="max-width:55%;margin-top:0.5rem">
<ul>
  <li><strong>penrose (gödel):</strong> podemos reconocer verdades que ningún sistema formal puede probar — luego no somos sistemas formales.</li>
  <li><strong>searle (cuarto chino):</strong> manipular símbolos correctamente no equivale a comprender. la semántica no surge de la sintaxis.</li>
  <li><strong>chalmers (problema difícil):</strong> ¿por qué hay algo que <em>se siente</em> ser yo? la experiencia subjetiva (qualia) no tiene correlato computacional obvio.</li>
  <li><strong>en resumen:</strong> que el cerebro sea físico no implica que lo importante de la mente sea computable.</li>
</ul>
</div>

---
layout: default
class: theme-frieren layer-vaporwave
---

# von neumann

<div style="display:grid;grid-template-columns:1fr 1.1fr;gap:3rem;margin-top:1.5rem;align-items:center">
<div>
<div class="big-quote theme-frieren-q" style="font-size:1.5rem">
  el problema no es si somos MT — es que no somos capaces de describir lo que hacemos.
</div>

<div style="margin-top:2rem">
<p style="font-size:1.3rem;line-height:1.6;opacity:0.8">
  para comparar una mente con una MT necesitaríamos primero escribir el algoritmo de la mente. y eso es exactamente lo que no podemos hacer.
</p>
<p style="font-size:1.3rem;line-height:1.6;opacity:0.8;margin-top:1rem">
  von neumann argumentaba que la barrera es <em>descriptiva</em>: no tenemos el lenguaje formal para siquiera formular bien la pregunta.
</p>
<p style="font-size:1.1rem;line-height:1.6;opacity:0.5;margin-top:1rem">
  no es que la respuesta sea "no" — es que la pregunta todavía no tiene forma precisa.
</p>
</div>
</div>

<img src="/img/11_vonneumann.png" style="width:100%;height:90%;object-fit:cover;border-radius:8px;opacity:0.78" onerror="this.style.display='none'" />
</div>

---
layout: default
class: theme-lain
---

# mi opinión

<div style="display:grid;grid-template-columns:1.2fr 1fr;gap:3rem;margin-top:1rem;align-items:start">
<div>
<p style="font-size:1.55rem;line-height:1.65;margin-bottom:1.5rem">
  sí. creo que somos equivalentes a una Máquina de Turing.
</p>
<p style="font-size:1.4rem;line-height:1.65;opacity:0.75">
  y no le veo nada de malo.
</p>
<p style="font-size:1.35rem;line-height:1.65;opacity:0.7;margin-top:1rem">
  no nos hace menos. no nos quita la risa, el dolor, el amor ni la rareza de existir.
</p>
<p style="font-size:1.3rem;line-height:1.65;opacity:0.6;margin-top:1rem">
  es como descubrir que el sol es una bola de gas. no deja de salir.
</p>
<p style="font-size:1.2rem;line-height:1.65;opacity:0.45;margin-top:1rem;font-family:var(--font-mono)">
  lo interesante no es la respuesta. es qué hacemos con ella.
</p>
</div>
<img src="/img/12_opinion.png" style="width:100%;height:90%;object-fit:cover;border-radius:8px;opacity:0.8" onerror="this.style.display='none'" />
</div>

---
layout: default
class: theme-vinland layer-solarpunk
---

<div style="position:absolute;inset:0;overflow:hidden">
  <img src="/img/13_solarpunk.png" style="width:100%;height:100%;object-fit:cover;opacity:0.55" onerror="this.style.display='none'" />
  <div style="position:absolute;inset:0;background:linear-gradient(to right, rgba(10,20,10,0.82) 42%, rgba(10,20,10,0.2) 100%)"></div>
</div>

<div style="position:relative;z-index:10;padding:3rem 3.5rem;height:100%;display:flex;flex-direction:column;justify-content:center;max-width:52%">
  <h1 style="font-size:3.8rem;font-weight:300;line-height:1.1;margin-bottom:2rem">y después</h1>

  <p style="font-size:1.45rem;line-height:1.7;opacity:0.9;margin-bottom:1.2rem">
    si somos computación — podemos elegir qué computar.
  </p>
  <p style="font-size:1.35rem;line-height:1.7;opacity:0.75;margin-bottom:1.2rem">
    saber cómo funcionamos no nos obliga a ningún futuro en particular. la pregunta no es qué somos, sino qué construimos con eso.
  </p>
  <p style="font-size:1.25rem;line-height:1.7;opacity:0.55;margin-bottom:1.2rem">
    una civilización que entiende sus propios límites computacionales puede diseñarse con más cuidado: más justa, más verde, más consciente de sus sesgos.
  </p>
  <p style="font-size:1.1rem;line-height:1.7;opacity:0.38;font-family:var(--font-mono)">
    church-turing no es un techo. es un mapa.
  </p>
</div>

---
layout: default
class: theme-lain
---

<div style="position:absolute;inset:0;display:flex;align-items:center;justify-content:center;gap:3rem;padding:2rem 3.5rem">
  <img src="/img/turing_meme.webp" style="max-height:86%;max-width:52%;object-fit:contain;border-radius:6px;flex-shrink:0" />
  <div style="display:flex;flex-direction:column;gap:1.4rem;max-width:38%">
    <p style="font-size:1.4rem;line-height:1.6;color:#e8fff3">el test de turing sigue siendo la mejor navaja de occam para la pregunta.</p>
    <p style="font-size:1.3rem;line-height:1.6;color:#7fffb5">si no puedes distinguirlo — ¿importa la respuesta?</p>
    <p style="font-size:1.1rem;line-height:1.6;color:#f7b500;font-family:var(--font-mono)">🏳️‍🌈 alan turing merece un futuro mejor que el que le dieron.</p>
    <div style="margin-top:0.5rem;display:flex;flex-direction:column;align-items:flex-start;gap:0.4rem">
      <QrCode
        value="https://uumami.wiki/presentacion_church_turing/"
        :size="110"
        background="#0b0f0d"
        foreground="#7fffb5"
      />
      <span style="font-family:var(--font-mono);font-size:0.65rem;color:#7fffb5;opacity:0.45;letter-spacing:0.06em">la tesis de church-turing</span>
    </div>
  </div>
</div>

---
layout: default
class: theme-lain layer-cyberpunk
---

<div class="close-wrap">
  <div class="close-title">
    <span class="handle" style="font-size:2.5rem;color:var(--lain-fg)">uumami</span>
    <div class="close-sub"><Slashes :items="['mario vázquez corte', 'dacc itam', '2026']" /></div>
  </div>

  <img src="/img/turing_meme.webp" style="max-height:72%;max-width:36%;object-fit:contain;border-radius:6px;opacity:0.92" />

  <div class="close-qr">
    <QrCode
      value="https://uumami.wiki/presentacion_church_turing/"
      :size="200"
      background="#0b0f0d"
      foreground="#7fffb5"
    />
    <div class="close-qr-label">la tesis de church-turing</div>
  </div>

  <div class="close-glyph">∞</div>
</div>

<style>
.close-wrap {
  position: absolute; inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 4rem;
  padding: 3rem 4rem;
}
.close-title {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}
.close-sub {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 0.95rem;
  opacity: 0.5;
  letter-spacing: 0.08em;
}
.close-qr {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.6rem;
}
.close-qr-label {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 0.78rem;
  color: #7fffb5;
  opacity: 0.5;
  letter-spacing: 0.06em;
}
.close-glyph {
  position: absolute;
  bottom: 3rem; right: 4rem;
  font-size: 12rem;
  color: #7fffb5;
  opacity: 0.04;
  font-family: 'IBM Plex Mono', monospace;
  user-select: none;
  line-height: 1;
}
</style>
