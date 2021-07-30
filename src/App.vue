<template>
  <modal-factory />
  <router-view />
</template>
<script>
import services from "./services";
import { watch } from "@vue/runtime-core";
import { useRoute, useRouter } from "vue-router";
import { setCurrentUser } from "./store/user"

import ModalFactory from "./components/ModalFactory";


export default {
  components: {
    ModalFactory,
  },

  setup() {
    const router = useRouter();
    const route = useRoute();

    watch(
      () => route.path,
      async() => {
        if (route.meta.hasAuth) {
          const token = window.localStorage.getItem("token");
          if (!token) {
            router.push({ name: "Home" });
            return;
          }
          const { data } = await services.users.getMe()
          setCurrentUser(data)
        }
      }
    );
  },
};
</script>