<template>
  <div class="trace-wrap">

    <!-- paso counter + estado -->
    <div class="trace-header">
      <div class="step-counter">
        <span class="k">paso</span>
        <span class="val">{{ step }} <span class="of">/ {{ steps.length - 1 }}</span></span>
      </div>
      <div class="state-pill" :class="current.stateClass">
        {{ current.state }}<span class="state-meaning"> · {{ current.stateMeaning }}</span>
      </div>
      <div class="input-label">entrada: <span class="mono">101</span> = 5  →  resultado: <span class="mono">110</span> = 6</div>
    </div>

    <!-- Cinta -->
    <div class="tape-zone">
      <div class="tape-label">cinta</div>
      <div class="tape-row">
        <div
          v-for="(cell, i) in current.tape"
          :key="i"
          class="cell"
          :class="{
            active: i === current.head,
            written: current.justWritten === i,
            blank: cell === '_',
          }"
        >
          <span class="sym">{{ cell }}</span>
          <div v-if="i === current.head" class="ptr">▲<span class="ptr-label">cabeza</span></div>
        </div>
        <div class="cell blank"><span class="sym">_</span></div>
        <div class="cell blank"><span class="sym">_</span></div>
        <div class="dots">···</div>
      </div>
    </div>

    <!-- Transición aplicada -->
    <div class="transition-box" v-if="step > 0">
      <span class="k">transición aplicada</span>
      <span class="mono trans">{{ current.transition }}</span>
    </div>
    <div class="transition-box init" v-else>
      <span class="k">configuración inicial</span>
      <span class="mono trans">q₀ = q_scan · cabeza en posición 0</span>
    </div>

    <!-- Explicación en español -->
    <div class="explanation">
      <span class="ex-icon">💬</span>
      <span class="ex-text">{{ current.explanation }}</span>
    </div>

    <!-- Controles -->
    <div class="trace-controls">
      <button class="btn" @click="prev" :disabled="step === 0">← anterior</button>
      <div class="progress-dots">
        <span
          v-for="(s, i) in steps"
          :key="i"
          class="dot"
          :class="{ active: i === step, done: i < step }"
          @click="step = i"
        ></span>
      </div>
      <button class="btn primary" @click="next" :disabled="step === steps.length - 1">
        {{ step === steps.length - 1 ? 'fin ✓' : 'siguiente →' }}
      </button>
    </div>

    <!-- Resultado final -->
    <div class="result-banner" v-if="step === steps.length - 1">
      <span>q_halt alcanzado · resultado en cinta:</span>
      <span class="mono big">110</span>
      <span class="mono dim">= 6 = 101 + 1 ✓</span>
    </div>

  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const step = ref(0)

const steps = [
  {
    tape: ['1','0','1','_','_'],
    head: 0,
    state: 'q_scan',
    stateMeaning: 'buscando el final',
    stateClass: 'scanning',
    justWritten: -1,
    transition: '',
    explanation: 'configuración inicial. la cabeza está en la primera casilla y lee "1". estoy en q_scan (modo búsqueda): tengo que avanzar → derecha hasta encontrar el blanco "_" que marca el final del número.',
  },
  {
    tape: ['1','0','1','_','_'],
    head: 1,
    state: 'q_scan',
    stateMeaning: 'buscando el final',
    stateClass: 'scanning',
    justWritten: 0,
    transition: 'δ(q_scan, 1) → (q_scan, 1, R)',
    explanation: 'leí "1". la regla dice: déjalo igual, muévete → derecha, sigue en q_scan. el "1" no me interesa todavía — mi única misión ahora es llegar al final.',
  },
  {
    tape: ['1','0','1','_','_'],
    head: 2,
    state: 'q_scan',
    stateMeaning: 'buscando el final',
    stateClass: 'scanning',
    justWritten: 1,
    transition: 'δ(q_scan, 0) → (q_scan, 0, R)',
    explanation: 'leí "0". igual que antes: lo dejo, avanzo → derecha, sigo en q_scan. no llegué al final todavía.',
  },
  {
    tape: ['1','0','1','_','_'],
    head: 3,
    state: 'q_scan',
    stateMeaning: 'buscando el final',
    stateClass: 'scanning',
    justWritten: 2,
    transition: 'δ(q_scan, 1) → (q_scan, 1, R)',
    explanation: 'leí otro "1", avanzo →. ahora la casilla siguiente es "_" (blanco) — eso es la señal que estoy buscando: el final del número.',
  },
  {
    tape: ['1','0','1','_','_'],
    head: 2,
    state: 'q_inc',
    stateMeaning: 'incrementando ←',
    stateClass: 'incr',
    justWritten: -1,
    transition: 'δ(q_scan, _) → (q_inc, _, L)',
    explanation: '¡encontré el blanco "_"! la regla dice: cambia a q_inc (modo incremento) y retrocede ← izquierda. ahora voy a sumar 1 desde el dígito menos significativo (el de más a la derecha).',
  },
  {
    tape: ['1','0','0','_','_'],
    head: 1,
    state: 'q_inc',
    stateMeaning: 'incrementando ←',
    stateClass: 'incr',
    justWritten: 2,
    transition: 'δ(q_inc, 1) → (q_inc, 0, L)',
    explanation: 'leo "1" en modo incremento. en binario: 1 + 1 = 10 — el resultado es 0 y llevo 1 (acarreo). escribo "0", me muevo ← izquierda, sigo en q_inc porque aún tengo acarreo pendiente.',
  },
  {
    tape: ['1','1','0','_','_'],
    head: 2,
    state: 'q_halt',
    stateMeaning: 'terminé',
    stateClass: 'halted',
    justWritten: 1,
    transition: 'δ(q_inc, 0) → (q_halt, 1, R)',
    explanation: 'leo "0" en modo incremento. en binario: 0 + 1 (acarreo) = 1 — escribo "1" y ya no hay más acarreo. la regla dice: cambia a q_halt. la máquina se detiene. resultado en cinta: 110 = 6 ✓',
  },
]

const current = computed(() => steps[step.value])

function next() { if (step.value < steps.length - 1) step.value++ }
function prev() { if (step.value > 0) step.value-- }
</script>

<style scoped>
.trace-wrap {
  display: flex;
  flex-direction: column;
  gap: 0.65rem;
  font-family: 'IBM Plex Mono', ui-monospace, monospace;
  background: rgba(11, 15, 13, 0.55);
  border: 1px solid rgba(127, 255, 181, 0.18);
  border-radius: 8px;
  padding: 1.1rem 1.4rem;
  pointer-events: all;
}

/* header */
.trace-header {
  display: flex;
  align-items: center;
  gap: 1.2rem;
}
.step-counter {
  display: flex;
  align-items: baseline;
  gap: 0.4rem;
}
.step-counter .k {
  font-size: 0.65rem;
  color: #7fffb5;
  opacity: 0.5;
  text-transform: uppercase;
  letter-spacing: 0.07em;
}
.step-counter .val {
  font-size: 1.4rem;
  color: #e8fff3;
  font-weight: 600;
}
.step-counter .of {
  font-size: 0.85rem;
  opacity: 0.4;
}
.state-pill {
  font-size: 0.8rem;
  padding: 0.15rem 0.7rem;
  border-radius: 999px;
  border: 1px solid;
  letter-spacing: 0.05em;
}
.state-pill.scanning { color: #2ef5ff; border-color: #2ef5ff; background: rgba(46,245,255,0.08); }
.state-pill.incr     { color: #f7b500; border-color: #f7b500; background: rgba(247,181,0,0.08); }
.state-pill.halted   { color: #7fffb5; border-color: #7fffb5; background: rgba(127,255,181,0.12); }
.state-meaning       { font-size: 0.72em; opacity: 0.65; }
.input-label {
  margin-left: auto;
  font-size: 0.78rem;
  color: #e8fff3;
  opacity: 0.4;
}
.input-label .mono { color: #7fffb5; opacity: 0.9; }

/* tape */
.tape-zone { display: flex; align-items: flex-start; gap: 0.8rem; }
.tape-label {
  font-size: 0.62rem;
  color: #7fffb5;
  opacity: 0.4;
  text-transform: uppercase;
  letter-spacing: 0.07em;
  padding-top: 0.8rem;
  min-width: 2.5rem;
}
.tape-row {
  display: flex;
  gap: 3px;
  align-items: flex-end;
}
.cell {
  position: relative;
  width: 2.6rem;
  height: 2.8rem;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid rgba(127, 255, 181, 0.2);
  border-radius: 4px;
  background: rgba(255,255,255,0.02);
  transition: all 0.2s;
  flex-shrink: 0;
}
.cell.blank .sym { opacity: 0.15; }
.cell.active {
  border-color: #7fffb5;
  background: rgba(127, 255, 181, 0.14);
  box-shadow: 0 0 14px rgba(127,255,181,0.3);
}
.cell.written {
  border-color: #f7b500;
  background: rgba(247,181,0,0.12);
  box-shadow: 0 0 10px rgba(247,181,0,0.2);
}
.cell .sym {
  font-size: 1.35rem;
  color: #e8fff3;
  font-weight: 600;
}
.ptr {
  position: absolute;
  bottom: -1.6rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
  color: #7fffb5;
  font-size: 0.75rem;
  line-height: 1;
}
.ptr-label {
  font-size: 0.55rem;
  opacity: 0.6;
  letter-spacing: 0.04em;
}
.dots {
  font-size: 1.2rem;
  color: #7fffb5;
  opacity: 0.2;
  padding: 0 0.3rem;
  align-self: center;
}

/* transition box */
.transition-box {
  display: flex;
  align-items: center;
  gap: 0.8rem;
  padding: 0.45rem 0.8rem;
  background: rgba(127,255,181,0.06);
  border-left: 2px solid rgba(127,255,181,0.4);
  border-radius: 0 4px 4px 0;
  margin-top: 0.8rem;
}
.transition-box.init { border-left-color: rgba(46,245,255,0.4); background: rgba(46,245,255,0.04); }
.transition-box .k {
  font-size: 0.65rem;
  color: #7fffb5;
  opacity: 0.5;
  text-transform: uppercase;
  letter-spacing: 0.06em;
  min-width: 10rem;
}
.transition-box .trans {
  font-size: 0.9rem;
  color: #7fffb5;
  font-weight: 500;
}

/* explanation */
.explanation {
  display: flex;
  gap: 0.7rem;
  align-items: flex-start;
  padding: 0.5rem 0.8rem;
  background: rgba(255,255,255,0.03);
  border-radius: 4px;
  border: 1px solid rgba(127,255,181,0.08);
}
.ex-icon { font-size: 1rem; line-height: 1.5; flex-shrink: 0; }
.ex-text {
  font-size: 0.88rem;
  color: #e8fff3;
  opacity: 0.82;
  line-height: 1.55;
  font-family: 'Inter', system-ui, sans-serif;
  font-weight: 300;
}

/* controls */
.trace-controls {
  display: flex;
  align-items: center;
  gap: 0.8rem;
}
.btn {
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(127,255,181,0.3);
  border-radius: 4px;
  color: #7fffb5;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 0.82rem;
  padding: 0.35rem 0.8rem;
  cursor: pointer;
  transition: all 0.12s;
  white-space: nowrap;
}
.btn:hover:not(:disabled) { background: rgba(127,255,181,0.12); }
.btn:disabled { opacity: 0.3; cursor: default; }
.btn.primary { border-color: #7fffb5; font-weight: 600; }
.btn.primary:hover:not(:disabled) { background: rgba(127,255,181,0.2); }

.progress-dots {
  display: flex;
  gap: 5px;
  flex: 1;
  justify-content: center;
}
.dot {
  width: 8px; height: 8px;
  border-radius: 999px;
  border: 1px solid rgba(127,255,181,0.35);
  cursor: pointer;
  transition: all 0.15s;
}
.dot.done { background: rgba(127,255,181,0.35); border-color: rgba(127,255,181,0.5); }
.dot.active { background: #7fffb5; border-color: #7fffb5; box-shadow: 0 0 6px rgba(127,255,181,0.6); }

/* result banner */
.result-banner {
  display: flex;
  align-items: center;
  gap: 0.8rem;
  padding: 0.5rem 1rem;
  background: rgba(127,255,181,0.1);
  border: 1px solid rgba(127,255,181,0.3);
  border-radius: 4px;
  font-size: 0.85rem;
  color: #7fffb5;
}
.result-banner .big { font-size: 1.3rem; font-weight: 700; }
.result-banner .dim { opacity: 0.5; }
</style>
