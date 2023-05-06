<template>
  <div class="flex flex-col">
    <h1 class="uppercase font-semibold text-3xl">user</h1>
    <hr />
    <h2 class="uppercase text-2xl">details</h2>
    <div class="flex flex-row justify-between">
      <h2 class="capitalize text-2xl">information</h2>
      <div class="px-3 py-1 bg-gray-200">
        <Icon icon="mdi:cog" class="h-5 w-5" />
      </div>
    </div>

    <table style="table-layout: fixed" class="border border-gray-200 mt-2">
      <tr>
        <th
          class="capitalize bg-gray-200 font-bold text-left px-2 border border-gray-300"
        >
          username
        </th>
        <td class="uppercase px-2 border border-gray-200">
          {{ user?.username }}
        </td>
      </tr>
      <tr>
        <th
          class="capitalize bg-gray-200 font-bold text-left px-2 border border-gray-300"
        >
          name
        </th>
        <td class="uppercase px-2 border border-gray-200">
          {{ user?.firstName }} - {{ user?.lastName }}
        </td>
      </tr>
      <tr>
        <th
          class="capitalize bg-gray-200 font-bold text-left px-2 border border-gray-300"
        >
          gender
        </th>
        <td class="uppercase px-2 border border-gray-200">
          {{ user?.gender }}
        </td>
      </tr>
      <tr>
        <th
          class="capitalize bg-gray-200 font-bold text-left px-2 border border-gray-300"
        >
          email
        </th>
        <td class="uppercase px-2 border border-gray-200">{{ user?.email }}</td>
      </tr>
      <tr>
        <th
          class="capitalize bg-gray-200 font-bold text-left px-2 border border-gray-300"
        >
          phone
        </th>
        <td class="uppercase px-2 border border-gray-200">{{ user?.phone }}</td>
      </tr>
    </table>

    <div class="flex justify-end mt-3">
      <button
        class="bg-gray-600 text-white px-2 rounded"
        @click="router.back()"
      >
        Back
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineProps, onMounted, reactive } from "vue";
import { Icon } from "@iconify/vue";
import { useRouter } from "vue-router";
const router = useRouter();
const props = defineProps({
  id: {
    type: String,
    required: true,
  },
});

type USER_TYPE = {
  username: string;
  firstName: string;
  lastName: string;
  email: string;
  phone: string;
  gender: string;
};

const user: USER_TYPE = reactive<USER_TYPE>({
  username: "",
  email: "",
  firstName: "",
  lastName: "",
  phone: "",
  gender: "",
});

onMounted(async () => {
  await fetchDetail(props.id);
});

const fetchDetail = async (id: string) => {
  let request = await fetch("https://dummyjson.com/users/" + id);
  if (!request.ok) {
    alert("Error getting user");
    return;
  }

  let json = await request.json();
  Object.assign(user, json);
};
</script>
