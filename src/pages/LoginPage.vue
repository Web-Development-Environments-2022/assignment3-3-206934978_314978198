<template>
  <div class="container">
    <LogInMainPage></LogInMainPage>
  </div>
</template>

<script>
import LogInMainPage from "../components/LogInMainPage.vue";

export default {
  name: "Login",
  components: {
    LogInMainPage,
  },
};
</script>
<style lang="scss" scoped>
.container {
  max-width: 400px;
}
</style>
