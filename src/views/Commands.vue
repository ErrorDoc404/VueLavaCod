<script setup>
import { onBeforeUnmount, onBeforeMount } from "vue";
import { useStore } from "vuex";
import Navbar from "@/examples/PageLayout/Navbar.vue";
const body = document.getElementsByTagName("body")[0];

const store = useStore();
onBeforeMount(async () => {
  store.state.hideConfigButton = true;
  store.state.showNavbar = false;
  store.state.showSidenav = false;
  store.state.showFooter = false;
  body.classList.remove("bg-gray-100");

  try {
    const response = await fetch('http://152.67.176.135:4040/api/commands'); // Replace with your API endpoint
    store.state.commands = await response.json();
  } catch (error) {
    console.error(error);
  }
});
onBeforeUnmount(() => {
  store.state.hideConfigButton = false;
  store.state.showNavbar = true;
  store.state.showSidenav = true;
  store.state.showFooter = true;
  body.classList.add("bg-gray-100");
});
</script>
<template>
  <div class="container top-0 position-sticky z-index-sticky">
    <div class="row">
      <div class="col-12">
        <navbar
          isBlur="blur  border-radius-lg my-3 py-2 start-0 end-0 mx-4 shadow"
          v-bind:darkMode="true"
          isBtn="bg-gradient-success"
        />
      </div>
    </div>
  </div>
  <main class="mt-0 main-content">
    <section>
      <div class="min-vh-100 mt-8">
        <div class="container">
          <div class="row justify-content-center">
            <div class="col-lg-3 col-md-6 d-flex flex-column mx-lg-0" v-for="(command, index) in store.state.commands" v-bind:key="index">
              <div class="card custom-card mb-4 mt-3">
                <div class="card-header">
                  <h4 class="font-weight-bolder">{{command.name}}</h4>
                  <p class="mb-0">{{command.description}}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>
