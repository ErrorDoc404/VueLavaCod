<script setup>
import { onBeforeUnmount, onBeforeMount, inject } from "vue";
import { useStore } from "vuex";
import Navbar from "@/examples/PageLayout/Navbar.vue";
import ArgonButton from "@/components/ArgonButton.vue";
const body = document.getElementsByTagName("body")[0];

const store = useStore();
onBeforeMount(async () => {
  store.state.hideConfigButton = true;
  store.state.showNavbar = false;
  store.state.showSidenav = false;
  store.state.showFooter = false;
  body.classList.remove("bg-gray-100");

  const socket = inject('socket');
  socket.emit('index');
  socket.on('index', (data) => {
    store.state.stats.songs = data.playMusic;
    store.state.stats.commands = data.commands;
  })

  try {
    const response = await fetch('https://echobyteapi.kartadharta.xyz/api/stats'); // Replace with your API endpoint
    store.state.stats = await response.json();
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

function openOptionWindow() {
  const screenWidth = screen.width;
  const screenHeight = screen.height;
  const windowWidth = 600;
  const windowHeight = 400;
  const left = (screenWidth - windowWidth) / 2;
  const top = (screenHeight - windowHeight) / 2;

  const windowFeatures = `width=${windowWidth},height=${windowHeight},menubar=no,toolbar=no,location=no,status=no,left=${left},top=${top}`;
  const optionWindow = window.open('about:blank', '_blank', windowFeatures);
  optionWindow.document.write('<html><body><h1>Option Window</h1><p>This is the option window content.</p></body></html>');
  optionWindow.document.close();
}
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
      <div class="page-header min-vh-100">
        <div class="container">
          <div class="row">
            <div class="mx-auto col-xl-4 col-lg-5 col-md-7 d-flex flex-column mx-lg-0">
              <div class="mb-3 card">
                <div class="p-3 card-body">
                  <div class="d-flex flex-row-reverse justify-content-between">
                    <div class="text-center shadow icon icon-shape bg-gradient-success rounded-circle">
                      <i class="text-lg opacity-10 ni ni-headphones" aria-hidden="true"></i>
                    </div>
                    <div class="">
                      <div class="numbers">
                        <p class="mb-0 text-sm text-uppercase font-weight-bold">Total Song</p>
                        <h5 class="mb-0 font-weight-bolder undefined">{{ store.state.stats ? store.state.stats.songs : 0 }} <!--v-if--><span class="text-sm font-weight-bolder text-success"></span></h5>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <div class="mb-3 card">
                <div class="p-3 card-body">
                  <div class="d-flex flex-row-reverse justify-content-between">
                    <div class="text-center shadow icon icon-shape bg-gradient-warning rounded-circle">
                      <i class="text-lg opacity-10 fa fa-server" aria-hidden="true"></i>
                    </div>
                    <div class="">
                      <div class="numbers">
                        <p class="mb-0 text-sm text-uppercase font-weight-bold">Total server</p>
                        <h5 class="mb-0 font-weight-bolder undefined">{{store.state.stats ? store.state.stats.guild : 0}} <!--v-if--><span class="text-sm font-weight-bolder text-success"></span></h5>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <div class="mb-3 card">
                <div class="p-3 card-body">
                  <div class="d-flex flex-row-reverse justify-content-between">
                    <div class="text-center shadow icon icon-shape bg-gradient-primary rounded-circle">
                      <i class="text-lg opacity-10 fa fa-terminal" aria-hidden="true"></i>
                    </div>
                    <div class="">
                      <div class="numbers">
                        <p class="mb-0 text-sm text-uppercase font-weight-bold">Total Commands</p>
                        <h5 class="mb-0 font-weight-bolder undefined">{{ store.state.stats ? store.state.stats.commands : 0 }} <!--v-if--><span class="text-sm font-weight-bolder text-success"></span></h5>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <argon-button
                class="mt-4"
                variant="gradient"
                color="success"
                fullWidth
                @click="openOptionWindow"
                size="lg"
              >Sign in via Discord</argon-button>
            </div>
            <div
              class="top-0 my-auto text-center col-6 d-lg-flex d-none h-100 pe-0 position-absolute end-0 justify-content-center flex-column"
            >
              <div
                class="position-relative bg-gradient-primary h-100 m-3 px-7 border-radius-lg d-flex flex-column justify-content-center overflow-hidden"
                style="
                  background-image: url(&quot;https://raw.githubusercontent.com/creativetimofficial/public-assets/master/argon-dashboard-pro/assets/img/signin-ill.jpg&quot;);
                  background-size: cover;
                "
              >
                <span class="mask bg-gradient-success opacity-6"></span>
                <h4
                  class="mt-5 text-white font-weight-bolder position-relative"
                >
                  "Attention is Music Bot"
                </h4>
                <p class="text-white position-relative">
                  &#x1F389; Bring the party to your Discord server with Echo Byte Music Bot! &#x1F389;
                  <br>Echo Byte is not just any music bot — it's your ultimate companion for immersive audio experiences in your community. Boasting a sleek interface and powerful features, Echo Byte turns your server into a vibrant hub of music and entertainment.
                  <br><br>
                  &#x1F680; Key Features: 
                  <ul>
                    <li class="text-white position-relative">Effortless Setup: Get started with easy installation and configuration in seconds.</li>
                    <li class="text-white position-relative">Vast Music Library: Access an extensive collection of tracks spanning all genres and eras.</li>
                    <li class="text-white position-relative">Customizable Commands: Tailor Echo Byte to suit your server's unique needs with customizable commands and settings.</li>
                    <li class="text-white position-relative">Seamless Integration: Ensure smooth music playback by Echo Byte commands.</li>
                    <li class="text-white position-relative">Rock-Solid Performance: Enjoy uninterrupted music streaming with Echo Byte's reliable and robust performance.</li>
                  </ul>
                  <br>
                  <span  class="text-white position-relative">From epic gaming sessions to laid-back hangouts, Echo Byte sets the mood with its dynamic playlist and crystal-clear sound quality.</span>
                  <br><br>
                  <span  class="text-white position-relative">&#x1F396; Ready to turn up the volume and ignite the atmosphere? Invite Echo Byte Music Bot to your server today and let the music take center stage! &#x1F396;</span>
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>