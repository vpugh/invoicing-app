<template>
  <div class="container-fluid body" style="padding: 0px;">
    <Header v-bind:user="user" v-bind:open="open" v-bind:title="title"/>
    <template v-if="this.isactive == 'create'">
      <CreateInvoice />
    </template>
    <template v-else>
      <ViewInvoices />
    </template>
  </div>
</template>

<script>
import Header from "./Header";
import CreateInvoice from "./CreateInvoice";
import ViewInvoices from "./ViewInvoices";
export default {
  name: "Dashboard",
  props: ["open"],
  components: {
    Header,
    CreateInvoice,
    ViewInvoices,
  },
  data() {
    return {
      isactive: 'create',
      title: "Invoicing App",
      user : (this.$route.params.user) ? this.$route.params.user : null
    };
  }
};
</script>

<style scoped>
.body {
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;
}
</style>
