<template>
  <div class="tm-wrap">
    <div class="tm-top">
      <span class="tm-title">máquina de turing</span>
      <span class="tm-desc">incremento binario · m = (Q, Γ, δ, q₀, F)</span>
      <span class="tm-badge" :class="stateClass">{{ stateLabel }}</span>
    </div>

    <!-- Tape -->
    <div class="tape-row">
      <div
        v-for="(cell, i) in visibleCells"
        :key="i"
        class="cell"
        :class="{
          active: i === headOffset,
          blank: cell === '_',
          written: writtenIdx === visibleStart + i,
        }"
      >
        <span class="sym">{{ cell }}</span>
        <div v-if="i === headOffset" class="head-tri">▲</div>
      </div>
    </div>

    <!-- State row -->
    <div class="info-row">
      <div class="info-box">
        <span class="k">estado</span>
        <span class="v">{{ currentState }}</span>
      </div>
      <div class="info-box">
        <span class="k">leyendo</span>
        <span class="v mono">{{ currentSymbol }}</span>
      </div>
      <div class="info-box">
        <span class="k">paso</span>
        <span class="v mono">{{ stepCount }}</span>
      </div>
    </div>

    <!-- δ aplicada + explicación en español -->
    <div class="explain-box" v-if="lastT || halted">
      <div class="explain-trans" v-if="lastT">
        <span class="k">δ</span><span class="mono">{{ lastT }}</span>
      </div>
      <div class="explain-text">{{ explanation }}</div>
    </div>
    <div class="explain-box init" v-else>
      <div class="explain-text">ingresa un número binario y presiona <strong>paso →</strong> para ejecutar una transición, o <strong>▶ run</strong> para correr hasta el final.</div>
    </div>

    <!-- Controls -->
    <div class="controls-row">
      <input
        v-model="inputVal"
        class="tape-input"
        placeholder="binario, ej: 1011"
        :disabled="running"
        @keyup.enter="reset"
      />
      <button class="btn" @click="reset" :disabled="running">↺</button>
      <button class="btn" @click="stepOnce" :disabled="halted || running">paso →</button>
      <button class="btn primary" @click="runAll" :disabled="halted || running">▶ run</button>
    </div>

    <!-- Transition table -->
    <div class="tt">
      <div class="tt-head">tabla de transición δ</div>
      <div
        v-for="(row, i) in TRANS"
        :key="i"
        class="tt-row"
        :class="{ lit: activeTrans === i }"
      >
        <span class="tt-from">δ({{ row.q }}, {{ row.r }})</span>
        <span class="tt-arr">→</span>
        <span class="tt-to">({{ row.q2 }}, {{ row.w }}, {{ row.d }})</span>
        <span class="tt-comment">// {{ row.note }}</span>
      </div>
    </div>

    <!-- Transition table -->
    <div class="tt">
      <div class="tt-head">tabla δ — haz clic en paso para ver cuál se activa</div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const B = '_'
const Q_SCAN = 'q_scan'
const Q_INC  = 'q_inc'
const Q_HALT = 'q_halt'

const TRANS = [
  { q: Q_SCAN, r: '0', q2: Q_SCAN, w: '0', d: 'R', note: 'sigue hacia la derecha' },
  { q: Q_SCAN, r: '1', q2: Q_SCAN, w: '1', d: 'R', note: 'sigue hacia la derecha' },
  { q: Q_SCAN, r: B,   q2: Q_INC,  w: B,   d: 'L', note: 'fin de cinta → incrementar' },
  { q: Q_INC,  r: '1', q2: Q_INC,  w: '0', d: 'L', note: 'carry: 1→0, continuar' },
  { q: Q_INC,  r: '0', q2: Q_HALT, w: '1', d: 'R', note: 'listo: 0→1, detener' },
  { q: Q_INC,  r: B,   q2: Q_HALT, w: '1', d: 'R', note: 'overflow: agregar 1' },
]

const inputVal   = ref('1011')
const tape       = ref([])
const head       = ref(0)
const currentState = ref(Q_SCAN)
const halted     = ref(false)
const running    = ref(false)
const log        = ref([])
const lastT      = ref('')
const activeTrans = ref(-1)
const writtenIdx = ref(-1)
const stepCount  = ref(0)

const WIN = 15
const visibleStart = computed(() => Math.max(0, head.value - Math.floor(WIN / 2)))
const visibleCells = computed(() =>
  Array.from({ length: WIN }, (_, i) => tape.value[visibleStart.value + i] ?? B)
)
const headOffset   = computed(() => head.value - visibleStart.value)
const currentSymbol = computed(() => tape.value[head.value] ?? B)

const STATE_MEANING = {
  [Q_SCAN]: 'buscando el final →',
  [Q_INC]:  'incrementando ←',
  [Q_HALT]: 'terminé',
}
const stateLabel = computed(() =>
  halted.value
    ? `q_halt · terminé`
    : `${currentState.value} · ${STATE_MEANING[currentState.value] ?? ''}`
)
const stateClass = computed(() => ({
  halted: halted.value,
  scanning: currentState.value === Q_SCAN && !halted.value,
  incr: currentState.value === Q_INC,
}))

const EXPLANATIONS = {
  [`${Q_SCAN}:0`]: 'leo "0" — no es el final. lo dejo igual, avanzo a la derecha. sigo en q_scan.',
  [`${Q_SCAN}:1`]: 'leo "1" — no es el final. lo dejo igual, avanzo a la derecha. sigo en q_scan.',
  [`${Q_SCAN}:_`]: 'leo "_" (blanco) — llegué al final del número. cambio a q_inc y retrocedo a la izquierda para empezar a incrementar.',
  [`${Q_INC}:1`]: 'leo "1" — en binario, 1+1=10. escribo "0" y llevo el acarreo: muevo a la izquierda y sigo en q_inc.',
  [`${Q_INC}:0`]: 'leo "0" — 0+1=1, no hay más acarreo. escribo "1", me muevo a la derecha y la máquina se detiene: q_halt.',
  [`${Q_INC}:_`]: 'leo "_" — el número era todo 1s (ej: 111→1000). insertar "1" al inicio. q_halt.',
}

const explanation = computed(() => {
  if (halted.value) {
    const result = tape.value.join('').replace(/^_+|_+$/g, '') || '0'
    return `máquina detenida. resultado en cinta: ${result}`
  }
  const sym = tape.value[head.value] ?? B
  return EXPLANATIONS[`${currentState.value}:${sym}`] ?? ''
})

function reset() {
  const raw = inputVal.value.replace(/[^01]/g, '') || '1011'
  tape.value = raw.split('').concat([B, B, B, B, B])
  head.value = 0
  currentState.value = Q_SCAN
  halted.value = false
  log.value = []
  lastT.value = ''
  activeTrans.value = -1
  writtenIdx.value = -1
  stepCount.value = 0
}

function stepOnce() {
  if (halted.value) return
  const sym = tape.value[head.value] ?? B
  const t = TRANS.findIndex(r => r.q === currentState.value && r.r === sym)
  if (t === -1) {
    halted.value = true
    log.value.push(`sin transición para δ(${currentState.value}, ${sym}) → halt`)
    return
  }
  const tr = TRANS[t]
  activeTrans.value = t

  // Extend tape
  while (tape.value.length <= head.value + 3) tape.value.push(B)

  tape.value[head.value] = tr.w
  writtenIdx.value = head.value
  lastT.value = `δ(${tr.q}, ${tr.r}) → (${tr.q2}, ${tr.w}, ${tr.d})`
  log.value.push(lastT.value)
  stepCount.value++

  currentState.value = tr.q2
  head.value += tr.d === 'R' ? 1 : -1

  if (head.value < 0) {
    tape.value.unshift(B)
    head.value = 0
  }

  if (currentState.value === Q_HALT) {
    halted.value = true
    const result = tape.value.join('').replace(/^_+|_+$/g, '') || '0'
    log.value.push(`resultado: ${result}`)
  }
}

function runAll() {
  running.value = true
  let steps = 0
  const iv = setInterval(() => {
    if (halted.value || steps > 60) {
      clearInterval(iv)
      running.value = false
      return
    }
    stepOnce()
    steps++
  }, 220)
}

reset()
</script>

<style scoped>
.tm-wrap {
  display: flex;
  flex-direction: column;
  gap: 0.7rem;
  font-family: 'IBM Plex Mono', ui-monospace, monospace;
  background: rgba(11, 15, 13, 0.6);
  border: 1px solid rgba(127, 255, 181, 0.2);
  border-radius: 8px;
  padding: 1.2rem 1.4rem;
  width: 100%;
  pointer-events: all;
}

/* top row */
.tm-top {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 0.2rem;
}
.tm-title {
  font-size: 0.85rem;
  color: #7fffb5;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}
.tm-desc {
  font-size: 0.75rem;
  color: #7fffb5;
  opacity: 0.45;
}
.tm-badge {
  margin-left: auto;
  font-size: 0.78rem;
  padding: 0.15rem 0.65rem;
  border-radius: 999px;
  border: 1px solid;
  letter-spacing: 0.06em;
}
.tm-badge.scanning { color: #2ef5ff; border-color: #2ef5ff; background: rgba(46,245,255,0.08); }
.tm-badge.incr     { color: #f7b500; border-color: #f7b500; background: rgba(247,181,0,0.08); }
.tm-badge.halted   { color: #7fffb5; border-color: #7fffb5; background: rgba(127,255,181,0.1); }

/* tape */
.tape-row {
  display: flex;
  gap: 2px;
  overflow: hidden;
  justify-content: center;
  padding: 0.3rem 0;
}
.cell {
  position: relative;
  width: 2.4rem;
  height: 2.6rem;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid rgba(127, 255, 181, 0.18);
  border-radius: 4px;
  background: rgba(255,255,255,0.02);
  transition: all 0.15s;
  flex-shrink: 0;
}
.cell.blank .sym { opacity: 0.15; }
.cell.active {
  border-color: #7fffb5;
  background: rgba(127, 255, 181, 0.12);
  box-shadow: 0 0 12px rgba(127,255,181,0.25);
}
.cell.written {
  border-color: #f7b500;
  background: rgba(247, 181, 0, 0.1);
}
.cell .sym {
  font-size: 1.2rem;
  color: #e8fff3;
  font-weight: 500;
}
.head-tri {
  position: absolute;
  bottom: -1.2rem;
  font-size: 0.7rem;
  color: #7fffb5;
  line-height: 1;
}

/* info row */
.info-row {
  display: flex;
  gap: 0.8rem;
  margin-top: 0.8rem;
}
.info-box {
  display: flex;
  flex-direction: column;
  gap: 0.1rem;
  min-width: 6rem;
}
.info-box.last-t { flex: 1; }
.info-box .k {
  font-size: 0.65rem;
  color: #7fffb5;
  opacity: 0.5;
  text-transform: uppercase;
  letter-spacing: 0.06em;
}
.info-box .v {
  font-size: 0.95rem;
  color: #e8fff3;
}
.info-box .v.mono { font-family: 'IBM Plex Mono', monospace; }
.info-box .v.small { font-size: 0.8rem; opacity: 0.85; }

/* explain box */
.explain-box {
  padding: 0.55rem 0.8rem;
  background: rgba(127,255,181,0.05);
  border-left: 2px solid rgba(127,255,181,0.4);
  border-radius: 0 4px 4px 0;
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}
.explain-box.init { border-left-color: rgba(46,245,255,0.3); background: rgba(46,245,255,0.04); }
.explain-trans {
  display: flex;
  align-items: center;
  gap: 0.6rem;
}
.explain-trans .k {
  font-size: 0.62rem;
  color: #7fffb5;
  opacity: 0.5;
  text-transform: uppercase;
  letter-spacing: 0.06em;
}
.explain-trans .mono {
  font-size: 0.82rem;
  color: #7fffb5;
  font-weight: 500;
}
.explain-text {
  font-size: 0.85rem;
  color: #e8fff3;
  opacity: 0.82;
  line-height: 1.5;
  font-family: 'Inter', system-ui, sans-serif;
  font-weight: 300;
}

/* controls */
.controls-row {
  display: flex;
  gap: 0.5rem;
  align-items: center;
  margin-top: 0.2rem;
}
.tape-input {
  flex: 1;
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(127,255,181,0.25);
  border-radius: 4px;
  color: #e8fff3;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 0.9rem;
  padding: 0.3rem 0.6rem;
  outline: none;
}
.tape-input:focus { border-color: rgba(127,255,181,0.6); }
.btn {
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(127,255,181,0.3);
  border-radius: 4px;
  color: #7fffb5;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 0.82rem;
  padding: 0.3rem 0.7rem;
  cursor: pointer;
  transition: all 0.12s;
  white-space: nowrap;
}
.btn:hover:not(:disabled) { background: rgba(127,255,181,0.12); }
.btn:disabled { opacity: 0.35; cursor: default; }
.btn.primary { border-color: #7fffb5; color: #7fffb5; font-weight: 600; }
.btn.primary:hover:not(:disabled) { background: rgba(127,255,181,0.2); }

/* transition table */
.tt {
  margin-top: 0.3rem;
}
.tt-head {
  font-size: 0.68rem;
  color: #7fffb5;
  opacity: 0.45;
  text-transform: uppercase;
  letter-spacing: 0.07em;
  margin-bottom: 0.3rem;
}
.tt-row {
  display: flex;
  gap: 0.6rem;
  align-items: center;
  font-size: 0.82rem;
  padding: 0.15rem 0.4rem;
  border-radius: 3px;
  transition: background 0.12s;
  color: #e8fff3;
  opacity: 0.55;
}
.tt-row.lit {
  background: rgba(127,255,181,0.1);
  opacity: 1;
  color: #7fffb5;
}
.tt-from { min-width: 11rem; }
.tt-arr { opacity: 0.4; }
.tt-to { min-width: 14rem; }
.tt-comment { opacity: 0.35; font-size: 0.75rem; }

/* log */
.log {
  border-top: 1px solid rgba(127,255,181,0.1);
  padding-top: 0.4rem;
  margin-top: 0.2rem;
}
.log-line {
  font-size: 0.78rem;
  color: #7fffb5;
  opacity: 0.65;
  line-height: 1.5;
}
</style>
