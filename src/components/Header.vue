<template>
  <div>
    <b-nav tabs align="center">
      <b-nav-item disabled
        ><h3>
          <b class="blue">Fancy </b><b class="green">Quiz </b>
          <b class="red">App</b>
        </h3></b-nav-item
      >
      <b-nav-item class="counter" disabled
        >Count: {{ numCorrect }}/ {{ numTotal }}</b-nav-item
      >
    </b-nav>
  </div>
</template>

<script>
export default {
  props: ["numCorrect", "numTotal"],
};
</script>

<style scoped>
.blue {
  color: #007bff;
}
.green {
  color: #28a745;
}
.red {
  color: #dc3545;
}
.counter {
  margin-top: 10px;
}
</style>
