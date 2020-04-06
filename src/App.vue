<template>
  <div id="app">
    <img alt="Vue logo" class="center" src="./assets/logo.png" />
    <HelloWorld msg="Welcome to Becalm v0.1" />
    <Sensor
      type="Temperatura"
      port="8888"
      endpoint="rest/api/v1.0/temperature" highErrMeasure=50 lowErrMeasure=10
    />
    <Sensor
      type="Presión PEEP"
      port="8888"
      endpoint="rest/api/v1.0/pressure" highErrMeasure=120000 lowErrMeasure=70000 />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import Sensor from "./components/Sensor.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
    Sensor
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 15%;
}
</style>
