<template>
  <div class="servers">
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
export default {
  name: "servers",
  data() {
    return {
      msg: "Hello Mars"
    };
  }
};
</script>

<style scoped>
.servers {
  color: #cccccc;
}
</style>