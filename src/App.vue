<template>
    <div id="app">
        <div class="office">
            <Map
                v-on:update:table-id="handleTableClick"/>
            <SideMenu :is-user-opened="isUserOpened"
                      v-on:update:is-user-opened="isUserOpened = $event"
                      :person="person"/>
        </div>
    </div>
</template>

<script>
import Map from "./components/Map.vue";
import SideMenu from "./components/SideMenu.vue";
import people from "@/assets/data/people.json";

export default {
    name: "App",
    components: {
        Map,
        SideMenu,
    },
    data() {
        return {
            tableId: null,
            isUserOpened: false,
            person: null,
        };
    },
    methods: {
        handleTableClick(tableId) {
            const person = people.find((p) => p.tableId === tableId);
            if (person) {
                this.person = person;
                this.isUserOpened = true;
            }
        },
    },
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    color: #2c3e50;
    background-color: #fafafa;
    padding: 24px;
    box-sizing: border-box;
}

html,
body,
#app {
    height: 100%;
}

* {
    box-sizing: border-box;
}

h3 {
    margin-top: 0px;
}

.office {
    display: grid;
    grid-template-columns: 1fr 320px;
    border-radius: 6px;
    border: 1px solid #ccd8e4;
    height: 100%;
    background: white;
    max-width: 1500px;
    margin: 0 auto;
}
</style>
