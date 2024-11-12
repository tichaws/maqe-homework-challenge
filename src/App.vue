<template>
  <div>
    <h1>MAQE Homework Challenge</h1>
    <h3>Candidate : Laphat-rada Wisadsri</h3>
  </div>
  <StylesComponent title="CSS Styling" />
  <TemplateComponent />
</template>
<script>
import StylesComponent from "./components/StylesComponent.vue";
import TemplateComponent from "./components/TemplateComponent.vue";

export default {
  name: "App",
  components: {
    StylesComponent,
    TemplateComponent,
  },
};
</script>

<style>
#app {
  font-family: sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
