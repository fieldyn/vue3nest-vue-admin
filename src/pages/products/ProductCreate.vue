<template>
  <form @submit.prevent="submit">
    <div class="mb-3">
      <label>Title</label>
      <input v-model="form.title" class="form-control" name="title" />
    </div>
    <div class="mb-3">
      <label>Description</label>
      <textarea v-model="form.description" class="form-control" name="description"> </textarea>
    </div>
    <div class="mb-3">
      <label>Image</label>
      <input v-model="form.image" class="form-control" name="image" />
    </div>
    <div class="mb-3">
      <label>Price</label>
      <input v-model="form.price" type="number" class="form-control" name="price" />
    </div>
    <button class="btn btn-outline-secondary">Save</button>
  </form>
</template>

<script lang="ts">
import { reactive } from "@vue/reactivity";
import axios from "axios";
import { useRouter } from "vue-router";
export default {
  name: "ProductCreate",
  setup() {
    const router = useRouter();
    const form = reactive({
      title: "",
      description: "",
      image: "",
      price: "",
    });

    const submit = async () => {
      await axios.post("products", form);

      router.push("/products");
    };

    return {
      form,
      submit,
    };
  },
};
</script>

<style lang="scss" scoped></style>
