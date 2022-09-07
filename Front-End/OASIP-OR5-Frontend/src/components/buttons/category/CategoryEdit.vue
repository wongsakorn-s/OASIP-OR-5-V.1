<script setup>
import { ref } from "vue";

defineEmits(["moreDetail", "editDetail"]);
const props = defineProps({
  detail: {
    type: Object,
    require: true,
  },
  category: {
    type: Array,
    require: true,
  },
  name: {
    type: String,
    require: true,
  },
  description: {
    type: String,
    require: true,
  },
  duration: {
    type: Number,
    require: true,
  },
});

const isModalOn = ref(false);
const isunique = ref(false);
const error = ref(false);

const unique = (name, id) => {
  props.category.forEach((e) => {
    if (id != e.id) {
      if (e.eventCategoryName == name) {
        isunique.value = true;
        error.value = true;
      }
    }
  });
};
</script>

<template>
  <button class="m-2 w-10 p-3 edit">
    <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10" fill="none" viewBox="0 0 24 24" stroke="currentColor"
      stroke-width="2" @click="$emit('moreDetail'); isModalOn = !isModalOn; error = false; name = detail.eventCategoryName; description = detail.eventCategoryDescriptionduration = detail.eventDuration;
      ">
      <path stroke-linecap="round" stroke-linejoin="round"
        d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z" />
    </svg>
  </button>

  <div v-show="isModalOn" class="modal-show flex justify-center">
    <div class="modal-content bg-base-100 rounded-2xl">
      <div class="flex justify-end">
        <!-- <button class="close" @click="isModalOn = !isModalOn">x</button> -->
        <button class="btn btn-circle btn-outline" @click="isModalOn = !isModalOn">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>
      <div class="flex justify-center">
        <div>
          <form method="post" @submit.prevent="$emit('editDetail', detail.id, name, description, duration, isunique);
          isunique == true ? isModalOn : (isModalOn = !isModalOn);
          isunique = false;">
            <div v-show="isModalOn" class="text-base font-medium grid justify-center py-2">
              <p class="grid justify-center font-bold text-4xl mb-4 font-header">
                Category Name
              </p>
              <input type="text" v-model="name" class="text-black p-1 m-1 rounded-md" maxlength="100" required />
              <p class="text-red-600" v-show="error">
                CategoryName must be unique!
              </p>
            </div>
            <div class="text-base font-medium grid justify-center py-2">
              <div v-show="isModalOn">
                <p class="grid justify-center font-bold text-2xl mb-4 font-header">
                  Description
                </p>
                <textarea cols="60" rows="4" maxlength="500" v-model="description"
                  class="text-black p-2 m-1 rounded-md"></textarea>
              </div>
            </div>
            <div class="text-lg font-medium grid justify-center py-2">
              <p>
                Duration :
                <input type="number" min="1" max="480" v-model="duration" class="text-black p-1 m-1 rounded-md"
                  required />
                <span>min: 1 | max: 480</span>
              </p>
            </div>
            <div class="flex justify-end">
              <input class="btn btn-active m-2" v-show="isModalOn" type="submit" value="OK"
                @click="unique(name, detail.id)" />
              <input class="btn btn-active m-2" v-show="isModalOn" type="button" value="Cancel"
                @click="isModalOn = !isModalOn" />
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.auto-fill {
  color: #8f8f8f;
}

.font-header {
  color: #2E86C1;
}

.modal-content {
  margin: auto;
  padding: 20px;
  width: 550px;
}

.modal-show {
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgb(32, 32, 32);
  background-color: rgba(73, 73, 73, 0.4);
}

.edit:hover {
  color: #3498DB;
}
</style>
