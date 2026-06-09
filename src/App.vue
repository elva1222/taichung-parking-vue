<script setup>
import TopNotice from './components/TopNotice.vue'
import SiteHeader from './components/SiteHeader.vue'
import TitleBar from './components/TitleBar.vue'
import InfoPanel from './components/InfoPanel.vue'
import QueryForm from './components/QueryForm.vue'
import SiteFooter from './components/SiteFooter.vue'
</script>

<template>
  <TopNotice />
  <SiteHeader />
  <TitleBar />

  <main class="page-wrap">
    <InfoPanel />
    <QueryForm />
  </main>

  <SiteFooter />
</template>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: "Microsoft JhengHei", Arial, sans-serif;
}

body {
  background: #e7e7e2;
  color: #333;
  font-size: 16px;
  line-height: 1.6;
}

a {
  text-decoration: none;
  color: inherit;
}

.page-wrap {
  max-width: 1180px;
  margin: 0 auto;
  padding: 18px 20px 36px;
}
</style>