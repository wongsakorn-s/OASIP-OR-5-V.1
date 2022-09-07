<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";

const email = ref();
const password = ref();
const error = ref(false);

const appRouter = useRouter();
const userRouter = () => appRouter.push({ name: "userList" });

//POST
const LoginUsers = async (email, password) => {
    const res = await fetch(`${import.meta.env.BASE_URL}api/users/match`, {
      method: "POST",
      headers: {
        "content-type": "application/json",
      },
      body: JSON.stringify({
        email: email,
        password: password
      }),
    });
    if (res.status === 200) {
	  alert("Password Matched")
    userRouter();
	  console.log("Login successfully");
      error.value = false;

    } else{
      error.value = true;
	  alert("Password NOT Matched")
      console.log("Error, User cannot login");
    }
  }

</script>

<template>
    <div class="flex flex-col items-center justify-center px-6 py-28 mx-auto">
        <div
            class="w-full bg-base-200 rounded-lg shadow dark:border md:mt-0 sm:max-w-md xl:p-0 dark:bg-gray-800 dark:border-gray-700">
            <div class="p-6 space-y-4 md:space-y-6 sm:p-8">
                <h1 class="text-center text-xl font-bold leading-tight tracking-tight text-gray-900 md:text-4xl dark:text-white">
                    Login
                </h1>
                <form action="post" @submit.prevent="LoginUsers(email, password);" class="space-y-4 md:space-y-6">
                    <div>
                        <label for="email" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">
                            Email
                        </label>
                        <input type="email" v-model="email"
                            class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                            placeholder="Your email">
                    </div>
                    <div>
                        <label for="password"
                            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Password</label>
                        <input type="password" v-model="password" placeholder="••••••••"
                            class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
                    </div>
                    <button type="submit"
                        class="w-full px-2 py-1.5 rounded-md bg-blue-600 hover:bg-blue-700 text-white mt-3">LOGIN</button>
                </form>
            </div>
        </div>
    </div>
</template>

<style scoped>
</style>
