<template>
  <header class="site-header">
    <div class="header-inner">
      <div class="brand-area">
        <div class="brand-main">e-Bill <span>全國繳費網</span></div>
        <div class="brand-sub">Safe | Quick | Convenient</div>
      </div>

      <nav class="top-nav">
        <a href="#">回首頁</a>
        <a href="#">網站導覽</a>
        <a href="#">問題與協助</a>
        <a href="#">自我環境檢測</a>
        <a href="#">繳費限額</a>
        <a href="#">費用搜尋</a>
      </nav>
    </div>
  </header>
</template>

<style scoped>
.site-header {
  background: #f7f7f7;
  border-bottom: 1px solid #d6d6d6;
}

.header-inner {
  max-width: 1180px;
  margin: 0 auto;
  min-height: 106px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 20px;
  padding: 18px 20px;
}

.brand-main {
  font-size: 28px;
  font-weight: 700;
  color: #9abd22;
}

.brand-main span {
  color: #70708c;
  margin-left: 8px;
}

.brand-sub {
  font-size: 14px;
  color: #737373;
  letter-spacing: 1px;
}

.top-nav {
  display: flex;
  flex-wrap: wrap;
  gap: 14px;
  color: #666;
  font-size: 14px;
}

@media (max-width: 900px) {
  .header-inner {
    flex-direction: column;
    align-items: flex-start;
  }
}
</style>