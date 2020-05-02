<template>
  <div class="row">
    <nav class="col s12 green darken-1">
      <div class="nav-wrapper">
        <router-link to="/" class="brand-logo">Tasks</router-link>
        <ul id="nav-mobile" class="right hide-on-med-and-down">
          <router-link to="/"
            tag="li"
            exact
            active-class="active"
          >
            <a href="#">Create</a>
          </router-link>
          <router-link to="/list"
            tag="li"
            active-class="active"
          >
            <a href="#">List</a>
          </router-link>
        </ul>
      </div>
    </nav>
  </div>
</template>

<script>
export default {
  
}
</script>

<style lang="scss">

</style>
