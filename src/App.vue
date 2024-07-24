<template>
  <div class="max-w-screen-xl mx-auto">
    <div class="flex justify-center items-center">
      <input
        type="text"
        id="search"
        class="border p-3 mt-5 rounded-md w-96 border-black"
        placeholder="Search..."
        @input="handleSearch"
      />
    </div>
    <div class="grid grid-cols-4 gap-3 gap-y-3">
      <div
        v-for="item in data"
        :key="item.id"
        class="max-w-sm rounded overflow-hidden shadow-lg"
      > 
        <div class="px-6 py-4">
          <div class="font-bold text-xl mb-2">{{ item.title }}</div>
          <p class="text-gray-700 text-base">
            {{ item.description }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import axios from "axios";
import { defineComponent, onMounted, ref } from "vue";

interface Product {
  id: number;
  title: string;
  description: string;
}

export default defineComponent({
  name: "TestingFetchAPI",
  setup() {
    const data = ref<Product[]>([]);
    const searchTerm = ref("");

    const fetchApi = async () => {
      try {
        const response = await axios.get(
          `https://dummyjson.com/products/search?q=${searchTerm.value}`
        );
        const responseData = response.data.products;
        console.log(responseData);
        data.value = responseData;
      } catch (error) {
        console.log(error);
      }
    };

    const handleSearch = (event: Event) => {
      const target = event.target as HTMLInputElement;
      console.log(target.value);
      searchTerm.value = target.value;
      fetchApi();
    };

    onMounted(() => {
      fetchApi();
    });

    return {
      data,
      handleSearch,
    };
  },
});
</script>
