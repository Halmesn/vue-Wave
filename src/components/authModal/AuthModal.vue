<template>
  <div
    class="fixed z-10 inset-0 overflow-y-auto"
    id="modal"
    :class="{ hidden: !authModalShow }"
  >
    <div
      class="
        flex
        items-end
        justify-center
        min-h-screen
        pt-4
        px-4
        pb-20
        text-center
        sm:block sm:p-0
      "
    >
      <div class="fixed inset-0 transition-opacity">
        <div class="absolute inset-0 bg-gray-800 opacity-75"></div>
      </div>

      <!-- This element is to trick the browser into centering the modal contents. -->
      <span class="hidden sm:inline-block sm:align-middle sm:h-screen"
        >&#8203;</span
      >

      <div
        class="
          inline-block
          align-bottom
          bg-white
          rounded-lg
          text-left
          overflow-hidden
          shadow-xl
          transform
          transition-all
          sm:my-8 sm:align-middle sm:max-w-lg sm:w-full
        "
      >
        <!-- Add margin if you want to see some of the overlay behind the modal-->
        <div class="py-4 text-left px-6">
          <!--Title-->
          <div class="flex justify-between items-center pb-4">
            <p class="text-2xl font-bold">Your Account</p>
            <!-- Modal Close Button -->
            <div
              class="modal-close cursor-pointer z-50"
              @click="toggleAuthModal"
            >
              <i class="fas fa-times"></i>
            </div>
          </div>

          <!-- Tabs -->
          <ul class="flex flex-wrap mb-4">
            <li class="flex-auto text-center" @click="tab = 'log'">
              <a
                class="block rounded py-3 px-4 transition"
                :class="{
                  'bg-blue-600 text-white hover:text-white': tab === 'log',
                  'hover:text-blue-600': tab === 'reg',
                }"
                href="#"
                >Login</a
              >
            </li>
            <li class="flex-auto text-center" @click="tab = 'reg'">
              <a
                class="block rounded py-3 px-4 transition"
                :class="{
                  'bg-blue-600 text-white hover:text-white': tab === 'reg',
                  'hover:text-blue-600': tab === 'log',
                }"
                href="#"
                >Register</a
              >
            </li>
          </ul>

          <!-- Login Form -->
          <LoginForm v-if="tab === 'log'" />
          <!-- Registration Form -->
          <RegisterForm v-else />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import { useStore } from 'vuex';
  import { computed, ref } from 'vue';
  import LoginForm from './LoginForm.vue';
  import RegisterForm from './RegisterForm.vue';

  export default {
    name: 'AuthModal',
    components: { LoginForm, RegisterForm },
    setup() {
      const store = useStore();

      store.dispatch('initLogin');
      const authModalShow = computed(() => store.state.auth.authModalShow);
      const toggleAuthModal = () => store.commit('toggleAuthModal');

      const tab = ref('log');

      return {
        tab,
        toggleAuthModal,
        authModalShow,
      };
    },
  };
</script>
