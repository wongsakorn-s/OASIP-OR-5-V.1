<script setup>
import { ref, onBeforeMount } from "vue";

import CategoryEdit from "./buttons/category/CategoryEdit.vue";
import CategoryNavbar from "./buttons/category/CategoryNavbar.vue";

const categories = ref([]);

// GET
const getCategories = async () => {
  const res = await fetch(`${import.meta.env.BASE_URL}api/eventCategory`);
  if (res.status === 200) {
    categories.value = await res.json();
  } else console.log("error, cannot get data");
};

onBeforeMount(async () => {
  await getCategories();
});

// PUT
const modifyCategories = async (id, newName, newDesc, newDuration, isunique) => {
  if (isunique == false) {
    const res = await fetch(`${import.meta.env.BASE_URL}api/eventCategory/${id}`, {
      method: "PUT",
      headers: {
        "content-type": "application/json",
      },
      body: JSON.stringify({
        eventCategoryName: newName,
        eventCategoryDescription: newDesc == null ? null : newDesc.trim(),
        eventDuration: newDuration,
      }),
    });
    if (res.status === 200) {
      getCategories();
      console.log("edited successfully");
    } else console.log("error, cannot edit");
  }
};

const currentDetail = ref({});
const data = ref({});
const moreDetail = (curbookingId) => {
  currentDetail.value = curbookingId;
  data.value = currentDetail.value;
};
</script>

<template>
  <div id="contents-list" class="px-10 py-5 flex justify-center">
    <table class="table-zebra table-layout table-element">
      <thead class="table-header bg-base-200">
        <tr>
          <CategoryNavbar />
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="contents in categories" :key="contents.id">
          <td class="p-10 text-xl">
            <div class="box-element break-words">
              {{ contents.eventCategoryName }}
            </div>
          </td>
          <td class="p-10 text-xl">
            <div v-if="
              contents.eventCategoryDescription != null &&
              contents.eventCategoryDescription.trim() != ''
            " class="pt-2">
              {{ contents.eventCategoryDescription }}
            </div>
            <div v-else-if="typeof contents.eventCategoryDescription" class="auto-fill text-xl font-medium">
              No message
            </div>
          </td>
          <td class="p-10 text-xl">{{ contents.eventDuration }} minute</td>

          <td>
            <div id="showDetail">
              <CategoryEdit @moreDetail="moreDetail(contents)" :detail="currentDetail"
                :name="currentDetail.eventCategoryName" :description="currentDetail.eventCategoryDescription"
                :duration="currentDetail.eventDuration" :category="categories" @editDetail="modifyCategories" />
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
table {
  text-align: left;
  position: relative;
  border-collapse: collapse;
  border-radius: 6px;
}

input,
textarea {
  color: rgb(0 0 0);
}

.table-header {
  position: sticky;
  top: 0;
  height: 100px;
}

.table-layout {
  table-layout: fixed;
  width: 90%;
}

.box-element {
  width: 250px;
}

.table-element {
  height: 100px;
}

.auto-fill {
  color: #8f8f8f;
}
</style>
