<template>
  <h2>Account Information</h2>
  <hr />
  <form @submit.prevent="infoSubmit">
    <div class="form-group">
      <label>First Name</label>
      <input type="text" class="form-control" name="first_name" v-model="infoData.first_name" />
    </div>
    <div class="form-group">
      <label>Last Name</label>
      <input type="text" class="form-control" name="last_name" v-model="infoData.last_name" />
    </div>
    <div class="form-group">
      <label>Email</label>
      <input type="text" class="form-control" name="email" v-model="infoData.email" />
    </div>

    <button class="btn btn-outline-secondary">Save</button>
  </form>

  <h2 class="mt-4">Change Password</h2>
  <hr />
  <form @submit.prevent="passwordSubmit">
    <div class="form-group">
      <label>Password</label>
      <input type="password" class="form-control" name="password" v-model="passwordData.password" />
    </div>
    <div class="form-group">
      <label>Password Confirm</label>
      <input type="password" class="form-control" name="password_confirm" v-model="passwordData.password_confirm" />
    </div>

    <button class="btn btn-outline-secondary">Save</button>
  </form>
</template>

<script lang="ts">
import { reactive } from "@vue/reactivity";
import { computed, watch } from "@vue/runtime-core";
import axios from "axios";
import { useStore } from "vuex";
export default {
  name: "Profile",
  setup() {
    const store = useStore();
    const user = computed(() => store.state.User.user);

    const infoData = reactive({
      first_name: "",
      last_name: "",
      email: "",
    });
    const passwordData = reactive({
      password: "",
      password_confirm: "",
    });

    watch(user, () => {
      infoData.first_name = user.value.first_name;
      infoData.last_name = user.value.last_name;
      infoData.email = user.value.email;
    });

    const infoSubmit = async () => {
      const { data } = await axios.put("users/info", infoData);
      await store.dispatch("User/setUser", data);
    };

    const passwordSubmit = async () => {
      await axios.put("users/password", passwordData);
    };

    return { infoData, passwordData, infoSubmit, passwordSubmit };
  },
};
</script>

<style lang="scss" scoped></style>
