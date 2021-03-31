<template>
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
import { ref, SetupContext, watch } from "@vue/runtime-core";
export default {
  name: "Paginator",
  emits: ["page-changed"],
  props: {
    lastPage: Number,
  },
  setup(props: { lastPage: number }, context: SetupContext) {
    const page = ref(1);

    watch(page, () => {
      context.emit("page-changed", page.value);
    });

    const next = () => {
      if (page.value < props.lastPage) {
        page.value++;
      }
    };
    const prev = () => {
      if (page.value > 1) {
        page.value--;
      }
    };
    return { next, prev };
  },
};
</script>
