<template>
  <section>
    <div class="mx-auto max-w-screen-xl px-4 py-8 sm:px-6 sm:py-12 lg:px-8">
      <header>
        <h2 class="text-xl font-bold text-gray-900 sm:text-3xl">Simple Tea</h2>
  
        <p class="mt-4 max-w-md text-gray-500">
          Tea experience menu
        </p>
      </header>
  
      <ul class="mt-8 grid gap-4 sm:grid-cols-2 lg:grid-cols-4" v-if="teas">
        <li v-for="tea in teas" :key="tea.uuid">
          <RouterLink :to="{ name: 'Tea', params: {id: tea.id} }" class="group block overflow-hidden">
            <img
              v-bind:src="tea.image[0]"
              alt=""
              class="h-[350px] w-full object-cover transition duration-500 group-hover:scale-105 sm:h-[450px]"
            />
  
            <div class="relative bg-white pt-3">
              <h3 class="text-xs text-gray-700 group-hover:underline group-hover:underline-offset-4">
                {{ tea.temperature }}
              </h3>
  
              <p class="mt-2">  
                <span class="tracking-wider text-gray-900"> {{ tea.name }} </span>
              </p>
            </div>
          </RouterLink>
        </li>
      </ul>
    </div>
  </section>
</template>
<script lang="ts">
import { ref, onMounted } from 'vue';
import axios from 'axios';
import { RouterLink } from 'vue-router';

interface Tea {
  uuid: string;
  id: string;
  name: string;
  temperature: string;
  ingredients: string[];
  image: string[];
}

export default {
    setup() {
        const teas = ref<Tea[] | null>(null);
        onMounted(async () => {
            const response = await axios.get('http://localhost:3000/tea');
            teas.value = response.data;
            console.log(teas.value)
        });
        return {
            teas
        };
    },
    components: { RouterLink }
};
</script>