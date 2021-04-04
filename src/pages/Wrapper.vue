<template>
  <Nav />
  <div class="container-fluid">
    <div class="row">
      <Menu />
      <main class="col-md-9 ms-sm-auto col-lg-10 px-md-4">
        <router-view />
      </main>
    </div>
  </div>
</template>

<script>
import Menu from "@/components/Menu.vue";
import Nav from "@/components/Nav.vue";
import axios from "axios";
import { onMounted } from "@vue/runtime-core";
import { useRouter } from "vue-router";
import { useStore } from "vuex";
export default {
  name: "Wrapper",
  components: { Menu, Nav },
  setup() {
    const router = useRouter();
    const store = useStore();

    onMounted(async () => {
      try {
        const { data } = await axios.get("user");

        await store.dispatch("User/setUser", data);
      } catch (ex) {
        await router.push("/login");
      }
    });
  },
};
</script>

<style lang="scss" scoped></style>
