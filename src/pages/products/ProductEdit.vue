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
      <div class="input-group">
        <input v-model="form.image" class="form-control" name="image" />
        <image-upload @uploaded="form.image = $event" />
      </div>
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
import { useRoute, useRouter } from "vue-router";
import ImageUpload from "@/components/ImageUpload.vue";
import { onMounted } from "@vue/runtime-core";
export default {
  components: { ImageUpload },
  name: "ProductEdit",
  setup() {
    const router = useRouter();
    const route = useRoute();
    const form = reactive({
      title: "",
      description: "",
      image: "",
      price: "",
    });

    onMounted(async () => {
      const { data } = await axios.get(`products/${route.params.id}`);

      form.title = data.title;
      form.description = data.description;
      form.image = data.image;
      form.price = data.price;
    });

    const submit = async () => {
      await axios.put(`products/${route.params.id}`, form);

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
