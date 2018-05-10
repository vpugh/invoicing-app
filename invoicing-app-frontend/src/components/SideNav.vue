<template>
    <div>
        <span style="font-size:30px;cursor:pointer" v-on:click="openNav">&#9776;</span>
        <div id="leftsidenav" class="sidenav" :class="{'open':open}">
            <p class="closeLink" v-on:click="closeNav"><em>Close Nav</em></p>
            <p><em>Company: {{ company }} </em></p>
            <h3>Welcome, {{ name }}</h3>
            <p class="clickable" v-on:click="setActive('create')">Create Invoice</p>
            <p class="clickable" v-on:click="setActive('view')">View Invoices</p>
        </div>
    </div>
</template>

<script>
export default {
  name: "SideNav",
  props: ["name", "company"],
  data() {
    return {
      active: "create",
      open: true
    };
  },
  methods: {
    setActive(option) {
      this.active = option;
      this.$parent.$parent.isactive = option;
    },
    openNav() {
      this.open = true;
    },
    closeNav() {
      this.open = false;
    }
  }
};
</script>

<style>
.sidenav {
  height: 100%;
  width: 300px;
  position: fixed;
  z-index: 1;
  top: 0;
  left: -300px;
  background-color: #fafafa;
  color: #818181;
  overflow-x: hidden;
  transition: 0.5s;
  text-align: center;
  padding-top: 60px;
}

.sidenav a {
  padding: 8px 8px 8px 32px;
  text-decoration: none;
  font-size: 25px;
  color: #818181;
  display: block;
  transition: 0.3s;
}

.sidenav a:hover {
  color: #f1f1f1;
}

.sidenav .closebtn {
  position: absolute;
  top: 0;
  right: 25px;
  font-size: 36px;
  margin-left: 50px;
}

.sidenav.open {
    left: 0;
}

.closeLink {
    font-size:12px;
    cursor:pointer;
    margin-bottom: 12px;
    font-weight: bold;
}

@media screen and (max-height: 450px) {
  .sidenav {
    padding-top: 15px;
  }
  .sidenav a {
    font-size: 18px;
  }
}

.clickable {
  cursor: pointer;
}
</style>
