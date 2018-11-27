<template>
  <RecordList />
</template>

<script>
import RecordList from "./components/RecordList.vue";

export default {
  name: "app",
  components: {
    RecordList
  }
};
</script>

<style lang="scss">
$darkblue: #24263f;
$blue: #2a4468;
$lightblue: #117cd9;
$white: #e7e9f5;
$red: #ed3224;
$purp: #24263f;

$heading: "Nunito", serif;
$subheading: "Raleway", sans-serif;
$copy: "Nunito", sans-serif;

html {
  background: $purp;
}

body {
  font-family: $copy;
  height: 100%;
  margin: 0;
  padding: 0;
  width: 100%;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  font-family: $heading;
}

* {
  box-sizing: border-box;
}
</style>
