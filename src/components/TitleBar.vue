<template>
  <section class="title-bar">
    <div class="title-inner">
      <div class="icon-circle">🚗</div>
      <div class="title-label">台中市停車費</div>
    </div>
  </section>
</template>

<style scoped>
.title-bar {
  background: #5f7900;
  padding: 10px 20px;
}

.title-inner {
  max-width: 1180px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  gap: 14px;
}

.icon-circle {
  width: 58px;
  height: 58px;
  border-radius: 50%;
  background: #f7f7f7;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 28px;
}

.title-label {
  background: #245ea7;
  color: #fff;
  font-size: 22px;
  font-weight: 700;
  padding: 8px 16px;
  border-radius: 3px;
}
</style>