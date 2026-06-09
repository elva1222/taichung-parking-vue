<script setup>
import { ref, onMounted } from 'vue'

const vehicleType = ref('汽車')
const plate = ref('')
const captchaInput = ref('')
const captchaCode = ref('')
const plateError = ref('')
const captchaError = ref('')
const showResult = ref(false)
const progressStep = ref(1)

const resultVehicleType = ref('-')
const resultPlate = ref('-')

const captchaCanvas = ref(null)

function randomNumberString(length) {
  let text = ''
  for (let i = 0; i < length; i++) {
    text += Math.floor(Math.random() * 10)
  }
  return text
}

function drawCaptcha() {
  captchaCode.value = randomNumberString(4)

  const canvas = captchaCanvas.value
  const ctx = canvas.getContext('2d')

  ctx.clearRect(0, 0, canvas.width, canvas.height)
  ctx.fillStyle = '#fffdf2'
  ctx.fillRect(0, 0, canvas.width, canvas.height)

  for (let i = 0; i < 16; i++) {
    ctx.strokeStyle = `hsl(${Math.random() * 360}, 70%, 60%)`
    ctx.beginPath()
    ctx.moveTo(Math.random() * canvas.width, Math.random() * canvas.height)
    ctx.lineTo(Math.random() * canvas.width, Math.random() * canvas.height)
    ctx.stroke()
  }

  for (let i = 0; i < captchaCode.value.length; i++) {
    const x = 24 + i * 38
    const y = 36 + Math.random() * 8
    const angle = (Math.random() - 0.5) * 0.5

    ctx.save()
    ctx.translate(x, y)
    ctx.rotate(angle)
    ctx.font = 'bold 36px Arial'
    ctx.fillStyle = i % 2 === 0 ? '#233cff' : '#7d3cff'
    ctx.fillText(captchaCode.value[i], 0, 0)
    ctx.restore()
  }

  for (let i = 0; i < 30; i++) {
    ctx.fillStyle = `rgba(0,0,0,${Math.random() * 0.2})`
    ctx.beginPath()
    ctx.arc(
      Math.random() * canvas.width,
      Math.random() * canvas.height,
      Math.random() * 2 + 1,
      0,
      Math.PI * 2
    )
    ctx.fill()
  }
}

function normalizePlate(value) {
  return value.trim().toUpperCase()
}

function isValidPlate(value) {
  const patterns = [
    /^[A-Z]{2,3}-\d{4}$/,
    /^\d{3,4}-[A-Z]{2,3}$/,
    /^[A-Z]{2,3}\d{4}$/,
    /^\d{3,4}[A-Z]{2,3}$/
  ]

  return patterns.some((pattern) => pattern.test(value))
}

function clearErrors() {
  plateError.value = ''
  captchaError.value = ''
}

function validateForm() {
  clearErrors()

  let isValid = true
  const normalizedPlate = normalizePlate(plate.value)
  const inputCaptcha = captchaInput.value.trim()

  if (!normalizedPlate) {
    plateError.value = '請輸入車牌號碼。'
    isValid = false
  } else if (!isValidPlate(normalizedPlate)) {
    plateError.value = '車牌格式錯誤，請輸入如 ABC-1234、AB-1234、123-ABC。'
    isValid = false
  }

  if (!inputCaptcha) {
    captchaError.value = '請輸入驗證碼。'
    isValid = false
  } else if (inputCaptcha !== captchaCode.value) {
    captchaError.value = '驗證碼錯誤，請重新輸入。'
    captchaInput.value = ''
    drawCaptcha()
    isValid = false
  }

  return isValid
}

function handlePlateInput() {
  plate.value = plate.value.toUpperCase()

  if (isValidPlate(normalizePlate(plate.value))) {
    plateError.value = ''
  }
}

function submitForm() {
  progressStep.value = 2

  if (!validateForm()) {
    progressStep.value = 1
    showResult.value = false
    return
  }

  resultVehicleType.value = vehicleType.value
  resultPlate.value = normalizePlate(plate.value)

  showResult.value = true
  progressStep.value = 3

  setTimeout(() => {
    document.querySelector('.result-box')?.scrollIntoView({
      behavior: 'smooth',
      block: 'start'
    })
  }, 100)
}

onMounted(() => {
  drawCaptcha()
})
</script>

<template>
  <section class="panel">
    <h2 class="panel-title">台中市停車查詢頁面</h2>

    <div class="progress-wrap">
      <div class="progress-step" :class="{ active: progressStep >= 1 }">
        <div class="step-circle">1</div>
        <div class="step-text">輸入查詢資料</div>
      </div>

      <div class="step-line"></div>

      <div class="progress-step" :class="{ active: progressStep >= 2 }">
        <div class="step-circle">2</div>
        <div class="step-text">確認資料</div>
      </div>

      <div class="step-line"></div>

      <div class="progress-step" :class="{ active: progressStep >= 3 }">
        <div class="step-circle">3</div>
        <div class="step-text">查詢結果</div>
      </div>
    </div>

    <div class="query-box">
      <div class="form-row">
        <div class="label">車輛種類 <span class="required">(必填)</span></div>
        <div class="content">
          <label>
            <input v-model="vehicleType" type="radio" value="汽車" />
            汽車
          </label>
          <label>
            <input v-model="vehicleType" type="radio" value="機車" />
            機車
          </label>
          <label>
            <input v-model="vehicleType" type="radio" value="微型電動二輪車" />
            微型電動二輪車
          </label>
        </div>
      </div>

      <div class="form-row">
        <div class="label">車牌號碼 <span class="required">(必填)</span></div>
        <div class="content plate-row">
          <input
            v-model="plate"
            type="text"
            class="text-input plate-input"
            :class="{ 'input-error': plateError }"
            placeholder="例如：ABC-1234"
            @input="handlePlateInput"
          />
          <p class="hint">
            請依車牌格式完整輸入，僅作為教學展示用途，請勿輸入真實資料。
          </p>
          <p class="field-error">{{ plateError }}</p>
        </div>
      </div>

      <div class="form-row">
        <div class="label">查詢說明</div>
        <div class="content note-text">
          本頁模擬停車費查詢畫面。送出後將顯示示範結果，不會進行任何真實帳單查詢。
        </div>
      </div>

      <div class="form-row">
        <div class="label">動態圖像驗證碼 <span class="required">(必填)</span></div>
        <div class="content">
          <div class="captcha-display-row">
            <canvas ref="captchaCanvas" width="190" height="58"></canvas>
            <button type="button" class="text-link" @click="drawCaptcha">重新產生</button>
          </div>

          <div class="captcha-input-wrap">
            <label class="captcha-label">請輸入上圖中看到的數字</label>
            <input
              v-model="captchaInput"
              type="text"
              class="text-input short-input"
              :class="{ 'input-error': captchaError }"
              placeholder="請輸入驗證碼"
              @input="captchaError = ''"
            />
            <p class="field-error">{{ captchaError }}</p>
          </div>
        </div>
      </div>

      <div class="button-row">
        <button type="button" class="btn btn-back">回上一頁</button>
        <button type="button" class="btn btn-submit" @click="submitForm">確認送出</button>
      </div>
    </div>
  </section>

  <section v-if="showResult" class="result-box">
    <h3>查詢結果（示範）</h3>

    <div class="result-status">
      <span class="status-badge">示範查詢完成</span>
      <span class="status-note">此區塊僅顯示前端模擬結果，不代表真實繳費資訊。</span>
    </div>

    <div class="result-grid">
      <div class="result-item">
        <div class="result-label">車輛種類</div>
        <div class="result-value">{{ resultVehicleType }}</div>
      </div>

      <div class="result-item">
        <div class="result-label">車牌號碼</div>
        <div class="result-value">{{ resultPlate }}</div>
      </div>

      <div class="result-item">
        <div class="result-label">查詢狀態</div>
        <div class="result-value">示範資料</div>
      </div>

      <div class="result-item">
        <div class="result-label">應繳金額</div>
        <div class="result-value">NT$ 120（展示）</div>
      </div>

      <div class="result-item">
        <div class="result-label">繳費期限</div>
        <div class="result-value">2026/04/30（展示）</div>
      </div>

      <div class="result-item">
        <div class="result-label">提醒事項</div>
        <div class="result-value">請以正式官方資訊為準</div>
      </div>
    </div>

    <div class="result-message">
      您目前看到的是教學展示結果。<br />
      本頁以台中市停車費查詢情境為主題，重現查詢頁常見的資訊編排、表單互動與結果呈現方式。<br />
      本系統不會連接任何真實官方平台，也不會送出、查詢或保存資料。<br />
      若收到真實停車費通知，請務必自行前往正式官方網站或服務窗口確認。
    </div>
  </section>
</template>

<style scoped>
.panel {
  background: #f9f9f9;
  border: 1px solid #d2d2d2;
  border-radius: 6px;
  padding: 18px 18px 22px;
  margin-bottom: 20px;
}

.panel-title {
  font-size: 24px;
  color: #5f7900;
  margin-bottom: 14px;
}

.progress-wrap {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 6px 10px 20px;
}

.progress-step {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-width: 120px;
}

.step-circle {
  width: 38px;
  height: 38px;
  border-radius: 50%;
  background: #c8c8c8;
  color: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
  font-weight: 700;
  margin-bottom: 8px;
}

.step-text {
  font-size: 14px;
  color: #666;
}

.progress-step.active .step-circle {
  background: #5f7900;
}

.progress-step.active .step-text {
  color: #5f7900;
  font-weight: 700;
}

.step-line {
  width: 80px;
  height: 2px;
  background: #cfcfcf;
  margin: 0 6px 24px;
}

.query-box {
  border: 1px solid #c8c8c8;
  background: #fff;
}

.form-row {
  display: grid;
  grid-template-columns: 240px 1fr;
  background: #edf0d7;
  border-bottom: 1px solid #d5d9bf;
}

.label {
  padding: 18px 16px;
  font-weight: 700;
  text-align: right;
}

.content {
  padding: 18px 16px;
}

.required {
  color: #e92b2b;
  font-weight: 700;
}

.content label {
  margin-right: 14px;
}

.text-input {
  height: 40px;
  border: 1px solid #9e9e9e;
  background: #fff;
  padding: 0 12px;
  font-size: 16px;
  outline: none;
}

.input-error {
  border-color: #d83b2d !important;
  background: #fff8f6;
}

.plate-row {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.plate-input,
.short-input {
  width: 260px;
  max-width: 100%;
}

.hint,
.note-text {
  color: #555;
  font-size: 15px;
}

.field-error {
  min-height: 22px;
  color: #d83b2d;
  font-size: 14px;
  font-weight: 700;
}

.captcha-display-row {
  display: flex;
  align-items: flex-start;
  gap: 14px;
  margin-bottom: 12px;
  flex-wrap: wrap;
}

canvas {
  border: 1px solid #cfcfcf;
  background: #fffdf2;
}

.text-link {
  border: none;
  background: transparent;
  color: #1e73be;
  font-size: 16px;
  cursor: pointer;
}

.captcha-input-wrap {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.button-row {
  display: flex;
  justify-content: center;
  gap: 14px;
  padding: 22px 16px 26px;
  background: #f4f4f4;
}

.btn {
  min-width: 140px;
  height: 44px;
  border: none;
  color: #fff;
  font-size: 16px;
  font-weight: 700;
  cursor: pointer;
  border-radius: 3px;
}

.btn-back {
  background: #666;
}

.btn-submit {
  background: #d86b1d;
}

.result-box {
  background: #fff;
  border-left: 6px solid #5f7900;
  padding: 18px 20px;
  margin-top: 20px;
  box-shadow: 0 3px 12px rgba(0, 0, 0, 0.08);
}

.result-box h3 {
  color: #5f7900;
  font-size: 24px;
  margin-bottom: 14px;
}

.result-status {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 12px;
  margin-bottom: 16px;
}

.status-badge {
  background: #5f7900;
  color: #fff;
  font-size: 14px;
  font-weight: 700;
  padding: 6px 12px;
  border-radius: 999px;
}

.status-note {
  font-size: 14px;
  color: #666;
}

.result-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 12px;
  margin-bottom: 18px;
}

.result-item {
  background: #f6f8eb;
  border: 1px solid #d8dcc3;
  padding: 14px;
  border-radius: 4px;
}

.result-label {
  font-size: 14px;
  color: #666;
  margin-bottom: 6px;
}

.result-value {
  font-size: 17px;
  font-weight: 700;
}

.result-message {
  font-size: 15px;
  line-height: 1.9;
  color: #444;
}

@media (max-width: 900px) {
  .form-row {
    grid-template-columns: 1fr;
  }

  .label {
    text-align: left;
    padding-bottom: 6px;
  }

  .result-grid {
    grid-template-columns: 1fr 1fr;
  }
}

@media (max-width: 600px) {
  .progress-wrap {
    flex-direction: column;
    gap: 10px;
  }

  .step-line {
    width: 2px;
    height: 24px;
    margin: 0;
  }

  .button-row {
    flex-direction: column;
    align-items: center;
  }

  .btn {
    width: 100%;
    max-width: 260px;
  }

  .result-grid {
    grid-template-columns: 1fr;
  }
}
</style>