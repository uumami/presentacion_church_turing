<template>
  <div class="rules-wrap">

    <!-- columna izquierda: glosario -->
    <div class="col-left">
      <p class="section-label">tarea: sumar 1 a un número binario</p>

      <div class="section-label" style="margin-top:0.7rem">ESTADOS (Q) — qué significa cada uno</div>
      <div class="state-list">
        <div class="state-item">
          <span class="state-name scan">q_scan</span>
          <span class="state-def"> — <strong>modo búsqueda</strong>: la cabeza avanza → hasta encontrar el final del número (el blanco "_")</span>
        </div>
        <div class="state-item">
          <span class="state-name inc">q_inc</span>
          <span class="state-def"> — <strong>modo incremento</strong>: suma 1 desde la derecha, propagando el acarreo ←</span>
        </div>
        <div class="state-item">
          <span class="state-name halt">q_halt</span>
          <span class="state-def"> — <strong>terminé</strong>: estado final. la máquina se detiene. el resultado está en la cinta</span>
        </div>
      </div>

      <div class="dir-box">
        <div class="section-label">DIRECCIÓN DE MOVIMIENTO</div>
        <div class="dir-row"><span class="dir-sym">L</span> = mover cabeza a la ← izquierda</div>
        <div class="dir-row"><span class="dir-sym">R</span> = mover cabeza a la → derecha</div>
      </div>
    </div>

    <!-- columna derecha: reglas δ -->
    <div class="col-right">
      <div class="section-label">REGLAS δ — qué hace la máquina en cada situación</div>
      <div class="rule-list">

        <div class="rule neutral">
          <div class="rule-formal">δ(q_scan, 0 ó 1) → (q_scan, mismo, R)</div>
          <div class="rule-plain">leo un dígito — lo dejo igual y avanzo → derecha. aún busco el final.</div>
        </div>

        <div class="rule amber">
          <div class="rule-formal">δ(q_scan, _) → (q_inc, _, L)</div>
          <div class="rule-plain">encontré el blanco: llegué al final. cambio a modo incremento y retrocedo ← a empezar a sumar.</div>
        </div>

        <div class="rule red">
          <div class="rule-formal">δ(q_inc, 1) → (q_inc, 0, L)</div>
          <div class="rule-plain">leo "1": en binario 1+1=10 — escribo 0, llevo el acarreo, sigo retrocediendo ←.</div>
        </div>

        <div class="rule green">
          <div class="rule-formal">δ(q_inc, 0) → (q_halt, 1, R)</div>
          <div class="rule-plain">leo "0": 0+1=1 — escribo 1, ya no hay acarreo. <strong>terminé: q_halt</strong>.</div>
        </div>

        <div class="rule dim">
          <div class="rule-formal">δ(q_inc, _) → (q_halt, 1, R)</div>
          <div class="rule-plain">caso especial: número era todo 1s (ej: 111 → 1000). inserto 1 al inicio.</div>
        </div>

      </div>
      <p class="example-line">ejemplo: <span class="mono amber">101</span> (= 5) → <span class="mono green">110</span> (= 6)</p>
    </div>

  </div>
</template>

<script setup></script>

<style scoped>
.rules-wrap {
  display: grid;
  grid-template-columns: 1fr 1.35fr;
  gap: 2.5rem;
  font-family: 'Inter', system-ui, sans-serif;
}

/* labels */
.section-label {
  font-family: 'IBM Plex Mono', ui-monospace, monospace;
  font-size: 0.78rem;
  color: #f7b500;
  letter-spacing: 0.07em;
  margin-bottom: 0.4rem;
  opacity: 0.85;
}
p.section-label { opacity: 0.5; margin: 0 0 0.3rem 0; }

/* left col */
.col-left { display: flex; flex-direction: column; gap: 0.5rem; }

.state-list { display: flex; flex-direction: column; gap: 0.55rem; }
.state-item { font-size: 1.05rem; line-height: 1.45; }
.state-name {
  font-family: 'IBM Plex Mono', monospace;
  font-weight: 600;
  font-size: 0.95rem;
}
.state-name.scan  { color: #f7b500; }
.state-name.inc   { color: #ff3a2f; }
.state-name.halt  { color: #7fffb5; }
.state-def { color: #e8fff3; opacity: 0.78; }

.dir-box {
  margin-top: 0.8rem;
  padding-top: 0.7rem;
  border-top: 1px solid rgba(255,255,255,0.08);
}
.dir-row {
  font-size: 1.05rem;
  color: #e8fff3;
  opacity: 0.78;
  line-height: 1.6;
}
.dir-sym {
  font-family: 'IBM Plex Mono', monospace;
  color: #f7b500;
  font-weight: 600;
  margin-right: 0.35rem;
}

/* right col */
.col-right { display: flex; flex-direction: column; gap: 0.4rem; }

.rule-list { display: flex; flex-direction: column; gap: 0.4rem; }
.rule {
  padding: 0.4rem 0.75rem;
  border-left: 2px solid;
  border-radius: 0 4px 4px 0;
}
.rule.neutral { background: rgba(255,255,255,0.03); border-color: rgba(255,255,255,0.15); }
.rule.amber   { background: rgba(247,181,0,0.07);   border-color: rgba(247,181,0,0.65); }
.rule.red     { background: rgba(255,58,47,0.07);   border-color: rgba(255,58,47,0.6); }
.rule.green   { background: rgba(127,255,181,0.07); border-color: rgba(127,255,181,0.6); }
.rule.dim     { background: rgba(255,255,255,0.02); border-color: rgba(255,255,255,0.1); }

.rule-formal {
  font-family: 'IBM Plex Mono', monospace;
  font-size: 0.75rem;
  opacity: 0.55;
  margin-bottom: 0.1rem;
}
.rule.amber  .rule-formal { color: #f7b500; opacity: 0.85; }
.rule.red    .rule-formal { color: #ff3a2f; opacity: 0.85; }
.rule.green  .rule-formal { color: #7fffb5; opacity: 0.85; }

.rule-plain {
  font-size: 0.98rem;
  color: #e8fff3;
  opacity: 0.82;
  line-height: 1.45;
}
.rule.dim .rule-plain { opacity: 0.45; }

.example-line {
  font-size: 0.95rem;
  opacity: 0.5;
  margin-top: 0.4rem;
  font-family: 'IBM Plex Mono', monospace;
}
.mono { font-family: 'IBM Plex Mono', monospace; }
.mono.amber { color: #f7b500; }
.mono.green { color: #7fffb5; }
</style>
