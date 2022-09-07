<script setup>
import moment from "moment";
import { ref } from "vue";

defineEmits(["moreDetail", "editDetail"]);
const props = defineProps({
  detail: {
    type: Object,
    require: true,
  },
  users: {
    type: Array,
    require: true,
  },
});

const edit = ref(false);
const isModalOn = ref(false);
const roles = ["student", "lecturer", "admin"];

const isunique = ref(false);
const Nerror = ref(false);

const uniquename = (name, id) => {
  props.users.forEach((e) => {
    if (e.id != id) {
      if (e.name.toLowerCase() == name.trim().toLowerCase()) {
        isunique.value = true;
        Nerror.value = true;
      }
    }
  });
};
const Eerror = ref(false);
const uniqueemail = (email, id) => {
  props.users.forEach((e) => {
    if (e.id != id) {
      if (e.email.toLowerCase() == email.trim().toLowerCase()) {
        isunique.value = true;
        Eerror.value = true;

      }
    }
  });
};
</script>

<template>
  <button class="m-2 w-10 p-3 edit">
    <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10" fill="none" viewBox="0 0 24 24" stroke="currentColor"
      stroke-width="2"
      @click="$emit('moreDetail'); isModalOn = !isModalOn; Nerror = false; Eerror = false; isunique = false; edit = false;">
      <path stroke-linecap="round" stroke-linejoin="round"
        d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z" />
    </svg>
  </button>

  <div v-show="isModalOn" class="modal-show flex justify-center">
    <div class="modal-content bg-base-100 rounded-2xl">
      <div class="flex justify-end">
        <button class="btn btn-circle btn-outline" @click="isModalOn = !isModalOn">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>
      <div class="flex justify-center">
        <div>
          <div class="text-3xl font-bold flex justify-center">
            <p v-show="!edit" class="font-header text-2xl font-bold py-2 grid justify-center">
              Name
            </p>
          </div>
          <div class="text-3xl font-bold flex justify-center">
            <p v-show="!edit" class="text-base font-medium grid justify-center py-2">
              {{ detail.name }}
            </p>
          </div>

          <div class="text-3xl font-bold flex justify-center">
            <p v-show="!edit" class="font-header text-2xl font-bold py-2 grid justify-center">
              Email
            </p>
          </div>

          <div v-show="!edit" class="text-base font-medium grid justify-center py-2">
            {{ detail.email }}
          </div>
          <div v-show="!edit">
            <p class="font-header text-2xl font-bold py-2 grid justify-center">
              Role
            </p>
            <div class="text-base font-medium grid justify-center py-2">
              {{ detail.role }}
            </div>
          </div>
          <!-- form -->
          <form method="post"
            @submit.prevent="$emit('editDetail', detail.id, detail.name, detail.email, detail.role, isunique); isunique == true ? edit : (edit = !edit); isunique = false;">
            <div v-show="edit" class="font-bold text-lg font-header">
              Name :
              <input type="text" v-model="detail.name" maxlength="100" class="text-black p-2 rounded-lg text-lg w-72" />
              <p class="text-red-600" v-show="Nerror">
                unique element
              </p>
            </div>
            <div v-show="edit" class="font-bold text-lg font-header py-5">
              Email :
              <input type="email" v-model="detail.email" maxlength="50"
                class="text-black p-2 rounded-lg text-lg w-72" />
              <p class="text-red-600" v-show="Eerror">
                unique element
              </p>
            </div>
            <div class="grid justify-center">
              <div v-show="edit" class="font-bold text-lg font-header">
                Role :
                <select name="role" class="text-black p-2 rounded-lg text-lg" v-model="detail.role" required>
                  <option v-for="role in roles">{{ role }}</option>
                </select>
              </div>
            </div>
            <p class="font-header text-2xl font-bold py-2 grid justify-center">
              Created on
            </p>
            <div class="text-base font-medium grid justify-center py-2">
              {{ moment(detail.createdOn).local().format("D MMMM YYYY, h:mm:ss A") }}
            </div>
            <p class="font-header text-2xl font-bold py-2 grid justify-center">
              Updated on
            </p>
            <div class="text-base font-medium grid justify-center py-2">
              {{ moment(detail.updatedOn).local().format("D MMMM YYYY, h:mm:ss A") }}
            </div>
            <div class="flex justify-center">
              <input class="btn btn-active m-2" v-show="edit" type="submit" value="OK"
                @click="uniquename(detail.name, detail.id); uniqueemail(detail.email, detail.id);" />
              <input class="btn btn-active m-2" v-show="edit" type="button" value="Cancel" @click="edit = !edit" />
            </div>
          </form>
        </div>
      </div>
      <div class="flex justify-center py-4">
        <button v-show="!edit" @click="edit = !edit" class="pl-2">
          <button class="btn btn-warning">Edit</button>
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
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
