<template>
    <nav class="navbar navbar-light bg-light push-60">
        <template v-if="user != null">
            <SideNav v-bind:name="user.name" v-bind:company="user.company_name"/>
            <span class="navbar-brand mb-0 h1">{{title}}</span>
        </template>
        <div class="profile">
            <a :href="'#'"><img class="profile-avatar"></a>
            <p class="mb-0 pr-4">Welcome, <a :href="'#'">{{user.name}}</a></p>
        </div>
    </nav>
</template>

<script>
import SideNav from './SideNav'
export default {
    name: "Header",
    props: ["user"],
    components: {
        SideNav
    },
    data() {
        return {
            title: "Invoicing App",
        };
    }
};
</script>

<style scoped>
.push-60 {
    margin-bottom: 60px;
}
.profile {
    display: grid;
    grid-template: auto / repeat(2,auto);
}
.profile-avatar {
    background-color: #ddd;
    border-radius: 50%;
    display: inline;
    width: 25px;
    height: 25px;
    margin-right: 20px;
}

</style>