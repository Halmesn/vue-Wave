<template>
  <header id="header" class="bg-gray-700">
    <nav class="container mx-auto flex justify-start items-center py-5 px-4">
      <!-- App Name -->
      <router-link
        class="text-white font-bold uppercase text-2xl mr-4"
        :to="{ name: 'home' }"
        exact-active-class="no-active"
        >Wave</router-link
      >

      <div class="flex flex-grow items-center">
        <!-- Primary Navigation -->
        <ul class="flex flex-row mt-1">
          <!-- Navigation Links -->
          <li>
            <router-link class="px-2 text-white" :to="{ name: 'about' }"
              >About</router-link
            >
          </li>
          <li v-if="!userLoggedIn">
            <a class="px-2 text-white" href="#" @click="toggleAuthModal"
              >Login / Register</a
            >
          </li>
          <template v-else>
            <li>
              <router-link class="px-2 text-white" :to="{ name: 'upload' }"
                >Upload songs</router-link
              >
            </li>

            <li>
              <a class="px-2 text-white" href="#" @click="signOut">Sign Out</a>
            </li>
          </template>
        </ul>
      </div>
    </nav>
  </header>
</template>

<script>
  import { useStore } from 'vuex';
  import { computed } from 'vue';
  import { useRouter, useRoute } from 'vue-router';

  export default {
    name: 'Header',
    setup() {
      const store = useStore();

      const userLoggedIn = computed(() => store.state.auth.userLoggedIn);
      const toggleAuthModal = () => store.commit('toggleAuthModal');

      const router = useRouter();
      const route = useRoute();

      const signOut = () => {
        store.dispatch('signOut', { route, router });
        route.meta.requireAuth && router.push({ name: 'home' });
      };

      return {
        toggleAuthModal,
        userLoggedIn,
        signOut,
      };
    },
  };
</script>
