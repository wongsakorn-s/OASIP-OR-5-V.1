<script></script>

<template>
  <div>
    <div class="bg-gray-100">
      <nav class="container px-6 py-8 mx-auto md:flex md:justify-between md:items-center">
        <div class="flex items-center justify-between">
          <router-link :to="{ name: 'home' }" class="text-4xl font-bold text-gray-800 md:text-4xl hover:text-blue-400">
            <svg class="h-8 fill-current inline" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512.005 512.005">
              <rect fill="#2a2a31" x="16.539" y="425.626" width="479.767" height="50.502"
                transform="matrix(1,0,0,1,0,0)" />
              <path class="plane-take-off"
                d=" M 510.7 189.151 C 505.271 168.95 484.565 156.956 464.365 162.385 L 330.156 198.367 L 155.924 35.878 L 107.19 49.008 L 211.729 230.183 L 86.232 263.767 L 36.614 224.754 L 0 234.603 L 45.957 314.27 L 65.274 347.727 L 105.802 336.869 L 240.011 300.886 L 349.726 271.469 L 483.935 235.486 C 504.134 230.057 516.129 209.352 510.7 189.151 Z " />
            </svg>
            OASIP
          </router-link>
        </div>

        <ul :class="showMenu ? 'flex' : 'hidden'"
          class="flex-col mt-8 space-y-4 md:flex md:space-y-0 md:flex-row md:items-center md:space-x-10 md:mt-0">
          <li class="text-sm font-bold text-gray-800 hover:text-blue-400">
            <router-link :to="{ name: 'home' }">Home</router-link>
          </li>
          <li class="text-sm font-bold text-gray-800 hover:text-blue-400">
            <router-link :to="{ name: 'categoryList' }">Category</router-link>
          </li>
          <li class="text-sm font-bold text-gray-800 hover:text-blue-400">
            <router-link :to="{ name: 'eventList' }">Schedule</router-link>
          </li>
          <li class="text-sm font-bold text-gray-800 hover:text-blue-400">
            <router-link :to="{ name: 'userList' }">User</router-link>
          </li>
          <li class="text-sm font-bold text-gray-800 hover:text-blue-400">
            <router-link :to="{ name: 'aboutUs' }">About Us</router-link>
          </li>
          <button class="btn gap-2">
            <router-link :to="{ name: 'loginUser' }">LOGIN</router-link>
          </button>
        </ul>
      </nav>
    </div>
  </div>
</template>

<style>
</style>
