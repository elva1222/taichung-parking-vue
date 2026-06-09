<template>
  <footer class="site-footer">
    <div class="footer-inner">
      <div class="footer-left">
        <div class="footer-logo">中華民國銀行公會</div>

        <div class="footer-links">
          <a href="#">關於 e-Bill</a>
          <a href="#">隱私權聲明</a>
          <a href="#">使用聲明</a>
          <a href="#">個資保護法告知</a>
          <a href="#">網路安全提醒</a>
        </div>

        <p>全國繳費網 版權所有 僅供教學展示用。</p>
      </div>

      <div class="footer-right">
        <div class="app-box">
          <div class="app-title">教學展示頁</div>
          <div class="qr-mock">QR</div>
        </div>
      </div>
    </div>
  </footer>
</template>

<style scoped>
.site-footer {
  background: #deded6;
  border-top: 1px solid #c7c7c7;
  padding: 20px;
}

.footer-inner {
  max-width: 1180px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  gap: 20px;
}

.footer-left {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.footer-logo {
  font-size: 22px;
  font-weight: 700;
  color: #7a5a2f;
}

.footer-links {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.footer-links a {
  border: 1px solid #b8b8b8;
  padding: 6px 10px;
  background: #f6f6f1;
  font-size: 14px;
}

.footer-left p {
  font-size: 14px;
  color: #555;
}

.app-box {
  width: 150px;
  background: #fff;
  border: 1px solid #bdbdbd;
  padding: 10px;
  text-align: center;
}

.app-title {
  font-size: 16px;
  margin-bottom: 8px;
  font-weight: 700;
}

.qr-mock {
  height: 100px;
  background: #fff;
  border: 1px dashed #333;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
}

@media (max-width: 900px) {
  .footer-inner {
    flex-direction: column;
    align-items: flex-start;
  }
}
</style>