<template>
  <div id="app">
    <Navbar />
    <div class="container">
      <div class="row">
        <div class="col s12">
          <router-view/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar";
export default {
  components: {
    Navbar
  }
}
</script>

<style lang="scss">
  @import "~materialize-css/dist/css/materialize.min.css"
</style>
