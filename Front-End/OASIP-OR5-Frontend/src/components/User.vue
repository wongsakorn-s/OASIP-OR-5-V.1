<script setup>
import UserNavbar from "./buttons/user/UserNavbar.vue";
import UserCreate from "./buttons/user/UserCreate.vue";
import UserDetail from "./buttons/user/UserDetail.vue";
import UserDelete from "./buttons/user/UserDelete.vue";

import { ref, onBeforeMount } from "vue";

const users = ref([]);

// GET
const getUsers = async () => {
  const res = await fetch(`${import.meta.env.BASE_URL}api/users`);
  if (res.status === 200) {
    users.value = await res.json();
  } else console.log("Error, cannot get data");
};
onBeforeMount(async () => {
  await getUsers();
});

// POST
const createNewUsers = async (Name, Email, Role, Password, isunique, error) => {
  if (Name.trim() != "" && isunique == false && error == false) {
    const res = await fetch(`${import.meta.env.BASE_URL}api/users`, {
      method: "POST",
      headers: {
        "content-type": "application/json",
      },
      body: JSON.stringify({
        name: Name.trim(),
        email: Email.trim(),
        role: Role,
        password: Password
      }),
    });
    if (res.status === 201) {
      getUsers();
      console.log("Created successfully");
    } else console.log("Error, User cannot be created");
  }
};

// DELETE
const removeUsers = async (id) => {
  if (confirm("Do you want to delete?")) {
    const res = await fetch(
      `${import.meta.env.BASE_URL}api/users/${id}`,
      {
        method: "DELETE",
      }
    );
    if (res.status === 200) {
      users.value = users.value.filter((users) => users.id !== id);
      console.log("Deleted successfullly");
    } else console.log("Error, User cannot be deleted");
  }
};

// PUT
const modifyUser = async (id, newName, newEmail, newRole, isunique) => {
  if (isunique == false) {
    const res = await fetch(`${import.meta.env.BASE_URL}api/users/${id}`, {
      method: "PUT",
      headers: {
        "content-type": "application/json",
      },
      body: JSON.stringify({
        name: newName,
        email: newEmail,
        role: newRole,
      }),
    });
    if (res.status === 200) {
      getUsers();
      console.log("Updated successfully");
    } else console.log("Error, User cannot be updated");
    console.log(newRole, newName, newEmail);
  }
};

const currentDetail = ref({});

const moreDetail = (curUserId) => {
  currentDetail.value = curUserId;
  getUsers();
};
</script>

<template>
  <div id="contents-list" class="px-10 py-5 flex justify-center">
    <table class="table-zebra table-layout table-element">
      <thead class="table-header bg-base-200">
        <tr>
          <UserNavbar />
          <th>
            <UserCreate @create="createNewUsers" :users="users" />
          </th>
        </tr>
      </thead>
      <div class="no-event text-5xl pt-20" v-if="users.length < 1">
        No Users
      </div>
      <tbody v-else>
        <tr v-for="contents in users">
          <td class="p-10 text-xl">
            <div class="box-element break-words">
              {{ contents.name }}
            </div>
          </td>
          <td class="p-10 text-xl">
            <div class="box-element break-words">
              {{ contents.email }}
            </div>
          </td>
          <td class="p-10 text-xl">
            <div class="box-element break-words">
              {{ contents.role }}
            </div>
          </td>
          <td>
            <div>
              <UserDetail @moreDetail="moreDetail(contents)" :detail="currentDetail" :users="users"
                @editDetail="modifyUser" />
              <UserDelete @delete="removeUsers(contents.id)" />
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

.table-element {
  height: 100px;
}

.table-layout {
  table-layout: fixed;
  width: 90%;
}

.table-header {
  position: sticky;
  top: 0;
  height: 100px;
}


.no-event {
  text-align: center;
  width: 100%;
  position: absolute;
  z-index: -1;
}

input,
textarea {
  color: rgb(0 0 0);
}

.box-element {
  width: 250px;
}
</style>
