<template>
  <div class="mx-auto max-w-screen-sm px-4 py-8 sm:px-6 sm:py-12 lg:px-8" v-if="tea">
    <a href="#" class="group block">
      <div class="relative h-[350px] sm:h-[450px]">
        <img
          v-bind:src="tea.image[0]"
          alt=""
          class="absolute inset-0 h-full w-full object-cover opacity-100 group-hover:opacity-0"
        />
    
        <img
          v-bind:src="tea.image[1]"
          alt=""
          class="absolute inset-0 h-full w-full object-cover opacity-0 group-hover:opacity-100"
        />
      </div>
    
      <div class="mt-3" >
        <h3 class="text-sm text-gray-700 group-hover:underline group-hover:underline-offset-4" >
          {{ tea.name }}
        </h3>
    
        <p class="mt-1.5 max-w-[40ch] text-xs text-gray-500">
          {{ tea.temperature }}
        </p>
        <p  class="mt-1.5 max-w-[40ch] text-xs text-gray-500" >
          <span class="mr-1" v-for="(ingredients, index) in tea.ingredients" v-bind:key="ingredients">
            {{ ingredients }}<span v-if="index != tea.ingredients.length-1" >,</span>
          </span>
        </p>
      </div>
    </a>
  </div>

  <div class="mx-auto max-w-screen-xl px-4 py-8 sm:px-6 sm:py-12 lg:px-8" v-if="otherTeas">
    <div class="grid grid-cols-4 gap-4">
      <a :href="$router.resolve({ name: 'Tea', params: {id: oneTea.id} }).href" class="group relative block bg-black" v-for="oneTea in otherTeas" :key="oneTea.uuid">
        <img
          alt=""
          v-bind:src="oneTea.image[0]"
          class="absolute inset-0 h-full w-full object-cover opacity-75 transition-opacity group-hover:opacity-50"
        />
      
        <div class="relative p-4 sm:p-6 lg:p-8">
      
          <p class="text-xl font-bold text-white sm:text-2xl">{{ oneTea.name }}</p>
      
          <div class="mt-32 sm:mt-48 lg:mt-64">
            <div
              class="translate-y-8 transform opacity-0 transition-all group-hover:translate-y-0 group-hover:opacity-100"
            >
              <p class="text-sm text-white">
                {{ oneTea.temperature }}
              </p>
            </div>
          </div>
        </div>
      </a>
    </div>
  </div>

</template>

<script lang="ts">
import { ref, onMounted } from 'vue';
import axios from 'axios';

interface Tea {
  uuid: string;
  id: string;
  name: string;
  temperature: string;
  ingredients: string[];
  image: string[];
}

export default {

  props: ['id'],
  setup(props) {
    const tea = ref<Tea | null>(null);
    const otherTeas = ref<Tea[] | null>(null);

    onMounted(async () => {
      const response = await axios.get(`http://localhost:3000/tea/${props.id}`);
      tea.value = response.data;

      const otherTeasResponse = await axios.get('http://localhost:3000/tea?_limit=4');
      otherTeas.value = otherTeasResponse.data;
    });
    return {
      tea,
      otherTeas
    };
  },
};
</script>