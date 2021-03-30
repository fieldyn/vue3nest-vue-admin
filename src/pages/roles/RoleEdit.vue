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
            :checked="checked(permission.id)"
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
import { useRoute, useRouter } from "vue-router";
import { onMounted } from "@vue/runtime-core";
import { Permission } from "@/models/permission";
export default {
  name: "RoleEdit",
  setup() {
    const router = useRouter();
    const route = useRoute();

    const formData = reactive<{ name: string; permissions: number[] }>({
      name: "",
      permissions: [],
    });
    const permissionList = ref([]);
    onMounted(async () => {
      const { data } = await axios.get("permissions");
      permissionList.value = data;

      const roleData = await axios.get(`roles/${route.params.id}`);
      formData.name = roleData.data.name;
      formData.permissions = roleData.data.permissions.map((p: Permission) => p.id);
    });
    const submit = async () => {
      await axios.put(`roles/${route.params.id}`, formData);
      await router.push("/roles");
    };
    const select = (id: number, checked: boolean) => {
      if (checked) {
        formData.permissions = [...formData.permissions, id];
        return;
      }

      formData.permissions = formData.permissions.filter((p) => p !== id);
    };
    const checked = (id: number) => formData.permissions.some((x) => x === id);

    return {
      formData,
      submit,
      select,
      permissionList,
      checked,
    };
  },
};
</script>

<style lang="scss" scoped></style>
