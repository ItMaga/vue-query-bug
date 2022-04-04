<template>
  <div id="app">
    <div v-if="isError">Error</div>
    <div v-else-if="isLoading">Loading...</div>
    <div v-else>{{ data }}</div>

    <button @click="invalidateQuery">Invalidate</button>
  </div>
</template>

<script>
import { useQueryProvider, useQuery, useQueryClient } from "vue-query";
import { ref, reactive } from "@vue/composition-api";

const mockApi = () => {
  return new Promise(resolve => {
    setTimeout(() => resolve('test'), 500);
  });
};

export default {
  name: 'App',

  setup() {
    useQueryProvider();
    const queryClient = useQueryClient();

    const page = ref(1);
    const queryKey = reactive(['test', { page }]);
    const { data, isLoading, isError } = useQuery(queryKey, mockApi);

    const invalidateQuery = () => queryClient.invalidateQueries(queryKey, { exact: true, active: true });

    return { data, isLoading, isError, invalidateQuery };
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
