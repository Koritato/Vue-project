<template>
  <div>
    <header class="header">
      <div class="head-container">
        <h1 class="logo">
          <a href="index.html">
            <img src="/logo.png" />
          </a>
        </h1>
      </div>
    </header>

    <main class="dashboard">
      <div class="left-panel">
        <section class="calculator-section">
          <div class="section-header">
            <h2>나의 적절 세팅</h2>
          </div>

          <div class="input-container">
            <div class="input-grid">
              <label for="height-slider">나의 신장 입력 (cm)</label>
              <div class="height-display">
                <span> {{ currentHeight }} </span>cm
              </div>
            </div>
            <input
              type="range"
              id="height-slider"
              min="150"
              max="200"
              step="1"
              v-model="currentHeight"
              :style="sliderStyle"
            />
            <div class="slider-range-labels">
              <span>150cm</span><span>160cm</span><span>170cm</span><span>180cm</span
              ><span>190cm</span><span>200cm</span>
            </div>
          </div>

          <div class="results-grid">
            <div class="result-card">
              <h3>의자 높이</h3>
              <div class="icon-box">
                <img src="/chair.png" class="res-icon" />
                <p>
                  <span>{{ results.chair }}</span> cm
                </p>
              </div>
            </div>
            <div class="result-card">
              <h3>책상 높이</h3>
              <div class="icon-box">
                <img src="/desk.png" class="res-icon" />
                <p>
                  <span>{{ results.desk }}</span> cm
                </p>
              </div>
            </div>
            <div class="result-card">
              <h3>모니터 높이</h3>
              <div class="icon-box">
                <img src="/monitor.png" class="res-icon" />
                <p>
                  <span>{{ results.monitor }}</span> cm
                </p>
              </div>
            </div>
          </div>

          <div class="ratio-selector-section">
            <h3>체형 비율 선택</h3>
            <div class="ratio-container">
              <div
                class="ratio-card"
                :class="{ active: currentRatioType === 'leg' }"
                @click="selectRatio('leg')"
              >
                <img src="/leg_dominant.png" alt="Leg Dominant" />
                <span class="ratio-desc">하체 긴 편</span>
              </div>
              <div
                class="ratio-card"
                :class="{ active: currentRatioType === 'balanced' }"
                @click="selectRatio('balanced')"
              >
                <img src="/balanced.png" alt="Balanced" />
                <span class="ratio-desc">표준 비율</span>
              </div>
              <div
                class="ratio-card"
                :class="{ active: currentRatioType === 'torso' }"
                @click="selectRatio('torso')"
              >
                <img src="/torso_dominant.png" alt="Torso Dominant" />
                <span class="ratio-desc">상체 긴 편</span>
              </div>
            </div>
          </div>
        </section>
      </div>

      <div class="right-panel">
        <div class="right-top-group">
          <section class="control-section">
            <div class="section-header">
              <h2>수동 제어</h2>
              <div class="setting">
                <button class="btn-up" @click="sendCommand('1')">
                  <span class="btn-icon">↑</span> &nbsp;UP
                </button>
                <button class="btn-down" @click="sendCommand('2')">
                  <span class="btn-icon">↓</span> &nbsp;DOWN
                </button>
                <button class="btn-forward" @click="sendCommand('3')">
                  <span class="btn-icon">+</span> &nbsp;FRONT
                </button>
                <button class="btn-backward" @click="sendCommand('4')">
                  <span class="btn-icon">-</span> &nbsp;BACK
                </button>
              </div>
            </div>
            <div class="control-actions">
              <button id="start-btn" class="btn btn-start" @click="sendCommand('5')">
                <span class="btn-icon">▶</span> START / PAUSE
              </button>
              <button id="stop-btn" class="btn btn-stop active" @click="sendCommand(7)">
                <span class="btn-icon">■</span> RESET , STOP
              </button>
            </div>
          </section>

          <section class="timer-section">
            <div class="section-header">
              <h2>실시간 모니터링</h2>
            </div>
            <div class="timer-group">
              <span class="timer-label">총 실행 시간</span>
              <div class="timer-value" id="total-runtime">00:00:00</div>
            </div>
            <div class="timer-group warning">
              <span class="timer-label">거북목 지속 시간</span>
              <div class="timer-value" id="turtle-runtime">00:00:00</div>
            </div>
          </section>
        </div>
        <section class="graph-monitoring-section">
          <div class="section-header">
            <h2>실시간 분석</h2>
            <div class="status-indicator">
              <span class="status-dot online"></span>
              <span class="status-text">LIVE</span>
            </div>
          </div>
          <div class="chart-container">
            <canvas id="realtime-chart"></canvas>
          </div>
        </section>
      </div>
    </main>
    <div class="second-panel">
      <section class="report-section">
        <div class="report-header">
          <h2>자세 분석</h2>
        </div>

        <div class="graph-main-container">
          <div class="graph-column">
            <div class="chart-box"><canvas id="chart-neck-angle"></canvas></div>
            <div class="chart-box"><canvas id="chart-cva"></canvas></div>
            <div class="chart-box"><canvas id="chart-shoulder"></canvas></div>
          </div>
          <div class="graph-column">
            <div class="chart-box"><canvas id="chart-head-tilt"></canvas></div>
            <div class="chart-box"><canvas id="chart-neck-turn"></canvas></div>
            <div
              style="
                flex: 2;
                display: flex;
                align-items: center;
                justify-content: center;
                opacity: 0.2;
              "
            >
              <img src="/logo.png" style="width: 200px" />
            </div>
          </div>
        </div>
      </section>

      <section class="feedback-section">
        <div class="feedback-header">
          <h2>자세 피드백</h2>
        </div>
        <div id="ai-feedback-content">데이터 분석을 대기 중입니다...</div>
        <button
          onclick="testAIFeedback()"
          style="
            background: #5ae3a3;
            color: #10141d;
            border: none;
            padding: 15px;
            border-radius: 8px;
            font-weight: bold;
            cursor: pointer;
            margin-top: 15px;
          "
        >
          AI 분석 실행
        </button>
      </section>
    </div>
  </div>
</template>

<script setup>
import { io } from 'socket.io-client'
import { onMounted, ref, computed } from 'vue'

// 1. 상태 변수 선언
const currentHeight = ref(178) // 초기 키 설정
const currentRatioType = ref('balanced') // 초기 체형 설정 ('balanced', 'leg', 'torso')

// 2. 체형 선택 함수
const selectRatio = (type) => {
  currentRatioType.value = type
}

// 3. 계산 로직
const results = computed(() => {
  const height = currentHeight.value

  let chair = height * 0.245
  let desk = chair + height * 0.135
  let monitor = desk + 30

  // 체형에 따른 보정
  if (currentRatioType.value === 'leg') {
    chair += 2.0
    monitor -= 0.7
  } else if (currentRatioType.value === 'torso') {
    chair -= 2.0
    desk += 1.5
    monitor += 1.5
  }

  return {
    chair: chair.toFixed(1),
    desk: desk.toFixed(1),
    monitor: monitor.toFixed(1),
  }
})

// 4. 슬라이더  스타일
const sliderStyle = computed(() => {
  const min = 150
  const max = 200
  const percentage = ((currentHeight.value - min) * 100) / (max - min)
  return {
    backgroundSize: `${percentage}% 100%`,
  }
})

// 5-1. 파이썬 서버와 연결
const socket = io('http://localhost:5000')

const serverMessage = ref('서버 연결 대기 중...')

onMounted(() => {
  // 서버로부터 응답이 올 때
  socket.on('server_response', (data) => {
    console.log('서버 응답:', data.msg)
    serverMessage.value = data.msg
  })
})

// 5-2. 버튼 클릭 시 실행
const sendCommand = (command) => {
  console.log('보내는 신호:', command)
  socket.emit('control_signal', command) // 'control_signal'으로 전송
}
</script>

<style>
/* 초기화 */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

body {
  background-color: #10141d;
  color: #ffffff;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

/* 상단 헤더  */
.header {
  width: 98%;
  height: 80px;
  background-color: #10141d;
  display: flex;
  align-items: center;
  border-bottom: 1px solid #2d3644;
}

.head-container {
  width: 100%;
  min-width: 1420px;
  margin: auto;
  padding: 0 30px;
  opacity: 0.8;
}

.logo img {
  height: 80px;
  width: auto;
}

.dashboard {
  display: flex;
  flex: 1;
  gap: 20px;
  width: 1300px;
  min-width: 1300px;
  margin: 0 auto;
  padding: 30px 20px;
}

.left-panel {
  flex: 0 0 600px;
  width: 80%;
}

.calculator-section {
  background-color: #1b212c;
  border-radius: 20px;
  padding: 20px;
  border: 1px solid #2d3644;
}

/* 결과 카드 및 기타 레이아웃 */
.input-grid {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
}

.height-display {
  background-color: #0c1016;
  padding: 8px 15px;
  border-radius: 8px;
  font-weight: bold;
}

/*  슬라이더 */
input[type='range'] {
  -webkit-appearance: none;
  width: 100%;
  height: 8px;
  background: #2d3644;
  border-radius: 10px;
  outline: none;
  background-image: linear-gradient(#96cef2, #5ae3a3);
  background-repeat: no-repeat;
  background-size: 50% 100%;
  cursor: pointer;
}

/* 슬라이더 손잡이 */
input[type='range']::-webkit-slider-thumb {
  -webkit-appearance: none;
  height: 20px;
  width: 20px;
  border-radius: 50%;
  background: #83aff7;
  cursor: pointer;
  border: 3px solid #ffffff;
  margin-top: -2px;
  transition: transform 0.2s ease;
}

.slider-range-labels {
  display: flex;
  justify-content: space-between;
  color: #6b7280;
  font-size: 0.9rem;
  margin-top: 8px;
}

.results-grid {
  display: flex;
  gap: 20px;
  margin-top: 20px;
  width: 100%;
  justify-content: space-between;
}

.result-card {
  flex: 1;
  background-color: #242b36;
  border: 1px solid #333d4d;
  border-radius: 8px;
  padding: 20px;
  text-align: center;
}
.result-card h3 {
  font-size: 0.9rem;
}
.result-card p span {
  color: #5ae3a3;
  font-size: 1.5rem;
  font-weight: bold;
}
/* 아이콘 자체 크기 조절 */
.res-icon {
  width: 120px; /* 원하는 크기로 조절 (기존보다 작게) */
  height: auto; /* 비율 유지 */
  margin-bottom: 10px; /* 아이콘과 아래 텍스트 사이 간격 */
  display: block; /* 중앙 정렬을 위해 필요 */
  margin-left: auto; /* 중앙 정렬 */
  margin-right: auto; /* 중앙 정렬 */
  opacity: 0.8; /* 너무 튀면 약간 투명하게 조절 가능 */
}

.ratio-container {
  display: flex;
  gap: 15px;
}

/* 비율 선택 섹션 */
.ratio-selector-section {
  margin-top: 20px;
  background-color: #1b212c;
  padding: 20px;
  border-radius: 15px;
  border: 1px solid #2d3644;
}

.ratio-selector-section h3 {
  font-size: 0.9rem;
  color: #9ca3af;
  margin-bottom: 15px;
}

.ratio-card {
  flex: 1;
  background-color: #0c1016;
  border: 1px solid #2d3644;
  border-radius: 10px;
  padding: 15px;
  text-align: center;
  cursor: pointer;
  transition: all 0.3s ease;
}

.ratio-card img {
  width: 60px;
  height: 40px;
  margin-bottom: 10px;
  opacity: 0.5;
  transition: opacity 0.3s ease;
  object-fit: cover;
}

.ratio-card span {
  display: block;
  font-size: 0.8rem;
  color: #6b7280;
}

.ratio-card.active {
  border-color: #5ae3a3;
  box-shadow: 0 0 15px rgba(90, 227, 163, 0.4);
  background-color: rgba(90, 227, 163, 0.05);
}

.ratio-card.active img {
  opacity: 1;
  filter: drop-shadow(0 0 5px #5ae3a3);
}

.ratio-card.active span {
  color: #5ae3a3;
  font-weight: bold;
}

.right-panel {
  flex: 0 0 680px;
  height: 350px;
  width: auto !important;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.right-top-group {
  display: flex;
  flex-direction: row; /* 가로 정렬 */
  gap: 20px;
  width: 100%;
  margin-bottom: -50px;
}

.control-section {
  background-color: #1b212c;
  border-radius: 20px;
  padding: 30px;
  border: 1px solid #2d3644;
  margin-bottom: 45px;
  width: 310px;
  height: 310px;
}

/* 버튼들을 감싸는 영역 (가로/세로 정렬을 위해 필요) */
.setting {
  display: grid;
  grid-template-columns: 1fr 1fr; /* 2열로 배치 */
  gap: 15px;
  margin-bottom: -6px;
  margin-top: 12px;
}

/* 개별 방향 버튼 스타일 (기존 .btn 스타일 활용) */
.setting button {
  background-color: #040404;
  border: 0px solid #ffffff;
  color: white;
  font: 8px;
  padding: 12px;
  width: 115px;
  height: 42px;
  border-radius: 25px;
  cursor: pointer;
  margin-top: 6px;
  font-weight: bold;
  transition: all 0.3s;
}

.setting button:hover {
  background-color: #b0b8c3;
  border: 0px solid #5ae3a3;
  color: black;
}

#stop-btn {
  display: flex;
  flex-direction: column; /* 아이콘과 텍스트를 세로로 나열 */
  justify-content: center; /* 세로 중앙 정렬 */
  align-items: center; /* 가로 중앙 정렬 (아이콘/텍스트 모두) */
  gap: 3px; /* 아이콘과 텍스트 사이 간격 */
  height: auto; /* 높이가 너무 낮으면 텍스트가 잘릴 수 있으니 확인 */
  padding: 10px 0;
  font-size: 0.8rem;
}

#start-btn {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 5px;
  height: auto;
  padding: 10px 0;
  font-size: 0.8rem;
}

#start-btn .btn-icon {
  font-size: 1.2rem; /* 아이콘 크기 살짝 키우기 (선택) */
  margin-bottom: -4px; /* 기존 마진 제거 */
}

#stop-btn .btn-icon {
  font-size: 1.2rem; /* 아이콘 크기 살짝 키우기 (선택) */
  margin-bottom: -2px; /* 기존 마진 제거 */
}

.control-actions {
  display: flex;
  gap: 15px;
  margin-top: 30px;
  width: 100%;
}

.btn {
  flex: 1;
  padding: 12px 0;
  border-radius: 12px;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  border: 2px solid transparent; /* 테두리 공간 확보 */
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

/* START 활성화 */
.btn-start.active {
  color: #ffffff;
  background: linear-gradient(145deg, #10b981, #059669);
  border-color: #34d399;
  box-shadow:
    0 0 20px rgba(16, 185, 129, 0.6),
    inset 0 0 10px rgba(255, 255, 255, 0.2);
}

/* STOP 활성화 */
.btn-stop.active {
  color: #ffffff;
  background: linear-gradient(145deg, #ef4444, #dc2626);
  border-color: #f87171;
  box-shadow:
    0 0 20px rgba(239, 68, 68, 0.6),
    inset 0 0 10px rgba(255, 255, 255, 0.2);
}

/* 비활성 버튼에 마우스 올렸을 때 반응 */
.btn:not(.active):hover {
  color: #fff;
}

.timer-section {
  background-color: #1b212c; /* 계산기 섹션과 동일한 배경색 */
  border-radius: 20px;
  padding: 30px;
  border: 1px solid #2d3644;
  height: 310px;
}

#timer-header {
  margin-top: -12px;
  margin-top: -12px;
}

.timer-group {
  display: flex;
  flex-direction: column;
  gap: 15px;
  margin-top: 20px;
  margin-bottom: -8px;
}

.timer-card {
  background-color: #0c1016;
  border: 1px solid #2d3644;
  padding: 20px;
  border-radius: 12px;
}

.timer-label {
  display: block;
  font-size: 12px;
  color: #a0aec0;
  margin-bottom: -14px;
  margin-top: -6px;
}

.timer-value {
  font-size: 28px;
  color: #5ae3a3;
  font-weight: bold;
  font-family: 'Courier New', Courier, monospace;
}

#turtle-runtime {
  color: #ffffff;
  font-weight: bold;
  font-family: 'Courier New', Courier, monospace;
}

/* 부모 박스 정렬 */
.icon-box,
.icon-placeholder {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 10px;
}

/* LIVE 상태 표시기 */
.status-indicator {
  display: flex;
  align-items: center;
  gap: 8px;
  background-color: #0c1016;
  padding: 5px 12px;
  border-radius: 20px;
  width: 10%;
}

.status-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
}

.status-dot.online {
  background-color: #5ae3a3; /* 네온 그린 */
  box-shadow: 0 0 10px rgba(90, 227, 163, 0.8);
  animation: pulse 1.5s infinite; /* 깜빡이는 효과 */
}

.status-text {
  font-size: 0.75rem;
  color: #888;
  font-weight: bold;
}

/* 그래프 컨테이너 */
.chart-container {
  width: 562px;
  height: 220px;
  margin-top: 20px;
  margin-bottom: 0px;
  position: relative;
}

/* 데이터 요약 스타일 */
.data-summary {
  display: flex;
  justify-content: flex-end;
  gap: 25px;
  border-top: 1px solid #2d3644;
  padding-top: 15px;
}

.summary-item {
  font-size: 0.9rem;
  color: #888;
}

.summary-item .value {
  color: #fff;
  font-weight: bold;
  font-size: 1.1rem;
  margin-right: 2px;
}

.summary-item .value.monitor-active {
  color: #5ae3a3; /* 네온 그린 */
}

/* 깜빡임 애니메이션 */
@keyframes pulse {
  0% {
    box-shadow: 0 0 0 0 rgba(90, 227, 163, 0.7);
  }
  70% {
    box-shadow: 0 0 0 10px rgba(90, 227, 163, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(90, 227, 163, 0);
  }
}

.timer-only {
  flex: 1; /* 타이머 박스 비율 */
  width: auto;
}

/* 하단 영역 전체 설정 */
.second-panel {
  margin: 60px auto; /* 상하 여백만 주고 좌우는 자동 중앙 정렬 */
  display: flex;
  gap: 20px;
  width: 100%;
  max-width: 1300px;
  padding: 0 20px;
  box-sizing: border-box;
  align-items: stretch;
}

.graph-main-container {
  display: flex;
  gap: 15px;
  margin-top: 15px;
}
/* 5개 그래프를 감싸는 박스 */
.report-section {
  background-color: #1b212c;
  border-radius: 20px;
  padding: 30px;
  border: 1px solid #2d3644;
  flex: 2.2; /* 오른쪽 피드백 섹션보다 2배 정도 넓게 설정 */
  min-height: 700px;
  display: flex;
  flex-direction: column;
}

.feedback-section {
  background-color: #1b212c; /* 기존 섹션들과 통일감 있는 색상 */
  border-radius: 20px;
  padding: 30px;
  border: 1px solid #2d3644;
  flex: 1 0 446px;
  display: flex;
  flex-direction: column;
  height: 700px;
  margin-right: 80px;
}

#ai-feedback-content {
  background: #0c1016;
  width: 100%;
  padding: 20px;
  color: #ffffff;
  border-radius: 12px;
  font-size: 16px;
  line-height: 1.6;
  flex: 1;
}

/* 그래프 배치 그리드 */
.graph-grid {
  display: flex;
  flex-direction: row;
  gap: 20px;
  margin-top: 20px;
  width: 200px;
  height: 100px;
}

.chart-box {
  background-color: #0c1016;
  border-radius: 12px;
  padding: 15px;
  height: 180px; /* 각 그래프 높이 */
  width: 100%;
}

.report-section {
  height: 660px;
  margin-bottom: 20px;
}
.graph-column {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 15px;
}
</style>
