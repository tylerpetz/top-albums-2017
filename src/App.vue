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
@import "bulma/bulma.sass";

$grey: #252525;

$font: "IBM Plex Mono", monospace;
$subheading: "Raleway", sans-serif;
$copy: "IBM Plex Mono", monospace;

html {
  background: $grey;
}

body {
  font-family: $font;
}
</style>
