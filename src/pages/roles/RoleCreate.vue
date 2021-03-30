<template>
  <form @submit.prevent="submit">
    <div class="mb-3 mt-3 row">
      <label class="col-sm-2 col-form-label">Name</label>
      <div class="col-sm-10">
        <input v-model="formData.name" type="text" class="form-control" name="name" />
      </div>
    </div>

    <div class="mb-3 row">
      <label class="col-sm-2 col-form-label">Permissions</label>
      <div class="col-sm-10">
        <div v-for="permission in permissionList" :key="permission.id" class="form-check form-check-inline col-3">
          <input
            type="checkbox"
            class="form-check-input"
            :value="permission.id"
            @change="select(permission.id, $event.target.checked)"
          />
          <label class="form-check-label">{{ permission.name }}</label>
        </div>
      </div>
    </div>
    <button class="btn btn-outline-secondary">Save</button>
  </form>
</template>

<script lang="ts">
import { reactive, ref } from "@vue/reactivity";
import axios from "axios";
import { useRouter } from "vue-router";
import { onMounted } from "@vue/runtime-core";
export default {
  name: "RoleCreate",
  setup() {
    const router = useRouter();

    const formData = reactive({
      name: "",
      permissions: [] as number[],
    });
    const permissionList = ref([]);
    onMounted(async () => {
      const { data } = await axios.get("permissions");
      permissionList.value = data;
    });
    const submit = async () => {
      await axios.post("roles", formData);
      await router.push("/roles");
    };
    const select = (id: number, checked: boolean) => {
      if (checked) {
        formData.permissions = [...formData.permissions, id];
        return;
      }

      formData.permissions = formData.permissions.filter((p) => p !== id);
    };
    return {
      formData,
      submit,
      select,
      permissionList,
    };
  },
};
</script>

<style lang="scss" scoped></style>
