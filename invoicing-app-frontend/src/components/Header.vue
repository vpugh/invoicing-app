<template>
    <nav class="navbar navbar-light bg-light" :class="{'isOpen': open}">
        <template v-if="user != null">
            <SideNav v-bind:name="user.name" v-bind:company="user.company_name"/>
        </template>
        <span class="navbar-brand mb-0 h1">{{title}}</span>
    </nav>
</template>

<script>
import SideNav from './SideNav'

export default {
  name: "Header",
  props : ["user", "title", "open"],
  components: {
    SideNav
  },
  data() {
    return {

    };
  }
};
</script>

<style scoped>
nav {
    margin-bottom: 60px;
}
</style>
