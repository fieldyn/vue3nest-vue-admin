<template>
  <div class="pt-3 pb-2 mb-3 border-bottom">
    <router-link to="/products/create" class="btn btn-sm btn-outline-secondary">Add</router-link>
  </div>
  <div class="table-responsive">
    <table class="table table-sm">
      <thead>
        <tr>
          <th>#</th>
          <th>Name</th>
          <th>Email</th>
          <th>Total</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <template v-for="order in orders" :key="order.id">
          <tr>
            <td>{{ order.id }}</td>
            <td>{{ order.name }}</td>
            <td>{{ order.email }}</td>
            <td>{{ order.total }}</td>
            <td>
              <div class="btn-group mr-2">
                <a href="javascript:void(0)" class="btn btn-sm btn-outline-secondary">View</a>
              </div>
            </td>
          </tr>
          <tr>
            <td colspan="5">
              <table class="table table-sm">
                <thead>
                  <tr>
                    <th>#</th>
                    <th>Name</th>
                    <th>Email</th>
                    <th>Total</th>
                    <th>Action</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="item in order.order_items" :key="item.id">
                    <td>{{ item.id }}</td>
                    <td>{{ item.product_title }}</td>
                    <td>{{ item.price }}</td>
                    <td>{{ item.quantity }}</td>
                  </tr>
                </tbody>
              </table>
            </td>
          </tr>
        </template>
      </tbody>
    </table>
  </div>
  <paginator :last-page="lastPage" @page-changed="load($event)" />
</template>

<script lang="ts">
import { onMounted, ref } from "@vue/runtime-core";
import axios from "axios";
import Paginator from "@/components/Paginator.vue";
export default {
  components: { Paginator },
  name: "Orders",
  setup() {
    const orders = ref([]);
    const lastPage = ref(0);

    const load = async (page = 1) => {
      const { data } = await axios.get(`orders?page=${page}`);
      orders.value = data.data;
      lastPage.value = data.meta.last_page;
    };
    onMounted(load);

    return { orders, lastPage, load };
  },
};
</script>

<style lang="scss" scoped></style>
