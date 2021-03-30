<template>
  <div class="table-responsive">
    <table class="table table-striped table-sm">
      <thead>
        <tr>
          <th>#</th>
          <th>Image</th>
          <th>Title</th>
          <th>Description</th>
          <th>Price</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="product in products" :key="product.id">
          <td>{{ product.id }}</td>
          <td><img :src="product.image" width="50px" /></td>
          <td>{{ product.title }}</td>
          <td>{{ product.description }}</td>
          <td>{{ product.price }}</td>
          <td>
            <div class="btn-group mr-2">
              <router-link :to="`/products/${product.id}/edit`" class="btn btn-sm btn-outline-secondary">
                Edit
              </router-link>
            </div>
            <div class="btn-group mr-2">
              <a href="javascript:void(0)" class="btn btn-sm btn-outline-secondary" @click="del(product.id)">Delete</a>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  <nav>
    <ul class="pagination">
      <li class="page-item">
        <a href="javascript:void(0)" class="page-link" @click="prev">Previous</a>
      </li>
      <li class="page-item">
        <a href="javascript:void(0)" class="page-link" @click="next">Next</a>
      </li>
    </ul>
  </nav>
</template>

<script lang="ts">
import { onMounted, ref, watch } from "@vue/runtime-core";
import axios from "axios";
import { Product } from "@/models/product";
export default {
  name: "Products",
  setup() {
    const products = ref([]);
    const page = ref(1);
    const lastPage = ref(0);

    const load = async () => {
      const { data } = await axios.get("products");
      products.value = data.data;
      lastPage.value = data.meta.last_page;
    };
    onMounted(load);

    watch(page, load);

    const next = () => {
      if (page.value < lastPage.value) {
        page.value++;
      }
    };
    const prev = () => {
      if (page.value > 1) {
        page.value--;
      }
    };
    const del = async (id: number) => {
      if (confirm("Are you sure?")) {
        await axios.delete(`products/${id}`);
        products.value = products.value.filter((p: Product) => p.id !== id);
      }
    };
    return { products, del, next, prev };
  },
};
</script>

<style lang="scss" scoped></style>
