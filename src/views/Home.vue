<template>
  <div class="flex flex-col gap-1">
    <h1 class="uppercase font-semibold text-3xl">user</h1>
    <hr />
    <div class="flex flex-row justify-between">
      <h2 class="uppercase text-2xl">list</h2>
      <div class="px-3 py-1 bg-gray-200">
        <Icon icon="mdi:cog" class="h-5 w-5" />
      </div>
    </div>
    <div class="flex flex-row justify-between">
      <input
        class="w-56 border border-gray-200 px-2"
        placeholder="Search"
        v-model="search"
      />
      <div class="flex fex-row">
        <Icon icon="mdi:package-down" class="h-10 w-10 text-gray-600" />
        <div class="px-3 py-1 bg-gray-600 text-white capitalize">
          invite user
        </div>
      </div>
    </div>

    <div v-if="filteredUsers.length > 0">
      <table style="table-layout: auto" class="border min-w-full">
        <thead>
          <tr class="bg-gray-200 border border-gray-300">
            <th class="uppercase border border-gray-300 text-left px-2">
              name
            </th>
            <th class="uppercase border border-gray-300 text-left px-2">
              email
            </th>
            <th class="uppercase border border-gray-300 text-left px-2">
              gender
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="user in filteredUsers" :key="user.id">
            <td class="uppercase border border-gray-300 px-2 text-blue-600">
              <span @click="navigateDetail(user.id)" class="cursor-pointer"
                >{{ user.firstName }} {{ user.lastName }}</span
              >
            </td>
            <td class="uppercase border border-gray-300 px-2">
              {{ user.email }}
            </td>
            <td class="uppercase border border-gray-300 px-2">
              {{ user.gender }}
            </td>
          </tr>
        </tbody>
      </table>

      <div class="flex flex-row justify-end mt-3">
        <div
          v-for="btn in paginateButton(
            paging.page,
            users.length / paging.perPage
          )"
        >
          <button
            class="border border-gray-600 w-7 h-7"
            :class="btn == paging.page ? 'bg-gray-600 text-white' : ''"
            @click="paging.page = btn"
          >
            {{ btn }}
          </button>
        </div>
      </div>
    </div>

    <div v-else class="text-center text-red-600 p-10">
      you don't have user data
    </div>
  </div>
</template>
<script setup lang="ts">
import { onMounted, reactive, ref, computed, watch } from "vue";
import { Icon } from "@iconify/vue";
import { useRouter } from "vue-router";

type USER_TYPE = {
  id: number;
  firstName: string;
  lastName: string;
  email: string;
  gender: string;
};
type PAGE_TYPE = {
  page: number;
  perPage: number;
};
const router = useRouter();
const users: USER_TYPE[] = reactive<USER_TYPE[]>([]);
const search = ref("");
const paging: PAGE_TYPE = reactive<PAGE_TYPE>({
  page: 1,
  perPage: 10,
});
const paginate = (items: USER_TYPE[], page = 1, perPage = 10) =>
  items.slice(perPage * (page - 1), perPage * page);

onMounted(async () => {
  await fetchUsers();
});

const fetchUsers = async () => {
  let request = await fetch("https://dummyjson.com/users");
  if (!request.ok) {
    alert("Error getting user");
    return;
  }

  let json = await request.json();
  users.splice(0);
  users.push(...json?.users);
};

const filteredUsers = computed(() => {
  let filter = paginate(users, paging.page, paging.perPage).filter(
    (u) =>
      u.firstName.toLowerCase().includes(search.value.toLowerCase()) ||
      u.lastName.toLowerCase().includes(search.value.toLowerCase()) ||
      u.email.toLowerCase().includes(search.value.toLowerCase()) ||
      u.gender.toLowerCase().includes(search.value.toLowerCase())
  );
  return filter;
});

const paginateButton = (current: number | any, total: number | any) => {
  const center = [current - 1, current, current + 1],
    filteredCenter = center.filter((p) => p > 1 && p < total),
    includeThreeLeft = current === 5,
    includeThreeRight = current === total - 4,
    includeLeftDots = current > 5,
    includeRightDots = current < total - 4;

  if (includeThreeLeft) filteredCenter.unshift(2);
  if (includeThreeRight) filteredCenter.push(total - 1);

  if (includeLeftDots) filteredCenter.unshift("...");
  if (includeRightDots) filteredCenter.push("...");

  return [1, ...filteredCenter, total];
};

const navigateDetail = (id: USER_TYPE["id"]) => {
  router.push({
    name: "detail",
    params: {
      id,
    },
  });
};
</script>
