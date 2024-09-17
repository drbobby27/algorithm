<template>
    <div class="max-w-lg mx-auto p-4">
      <h1 class="text-2xl font-bold mb-4">Algorithm</h1>
  
      <div class="mb-4">
        <label for="numbers" class="block text-lg font-medium">
          Base list of numbers (separated by a simple space):
        </label>
        <input
          id="numbers"
          v-model="numbersInput"
          type="text"
          class="mt-1 p-2 border border-gray-300 rounded-lg w-full"
          placeholder="Input integer numbers: 2 4 5 7 9"
        />
      </div>
  
      <div class="mb-4">
        <label for="queries" class="block text-lg font-medium">
          Input a query
        </label>
        <input
          id="queries"
          v-model="queriesInput"
          type="text"
          class="mt-1 p-2 border border-gray-300 rounded-lg w-full"
          placeholder="Input integer numbers: 2 5 6 10"
        />
      </div>
  
      <div class="flex justify-center mb-6">
        <button
          @click="getResult"
          class="bg-blue-500 text-white px-4 py-2 rounded-lg hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-opacity-50"
        >
          Check
        </button>
      </div>
  
      <div v-if="results.length > 0" class="mt-6">
        <h2 class="text-xl font-semibold mb-2">Results</h2>
        <ul class="list-disc pl-5 space-y-1">
          <li v-for="(result, index) in results" :key="index" class="text-lg">
            {{ result }}
          </li>
        </ul>
      </div>
  
      <div v-if="errorMessage" class="mt-4 p-4 bg-red-100 text-red-700 border border-red-300 rounded-lg">
        {{ errorMessage }}
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  
  const numbersInput = ref<string>('');
  const queriesInput = ref<string>('');
  const results = ref<string[]>([]);
  const errorMessage = ref<string | null>(null);
  
  const cleanInput = (input: string): string => {
    return input.replace(/\s+/g, ' ').trim();
  };
  
  const isValidNumberArray = (input: string): boolean => {
    return input.split(' ').every(num => !isNaN(parseInt(num, 10)));
  };
  
  const isValidInteger = (num: any): boolean => {
    return Number.isInteger(num);
  };
  
  const findCloseNumber = (list: number[], queries: number[]): string[] => {
    return queries.map(q => {
      let lower: number | 'X' = 'X';
      let higher: number | 'X' = 'X';
  
      for (const num of list) {
        if (num < q) {
          lower = num;
        } else if (num > q) {
          if (higher === 'X') {
            higher = num;
          }
        }
      }
  
      return `${lower} ${higher}`;
    });
  };
  
  const getResult = () => {
    numbersInput.value = cleanInput(numbersInput.value);
    queriesInput.value = cleanInput(queriesInput.value);
  
    if (!isValidNumberArray(numbersInput.value)) {
      errorMessage.value = 'Invalid list of numbers. Please enter only valid integers separated by spaces.';
      results.value = [];
      return;
    }
  
    if (!isValidNumberArray(queriesInput.value)) {
      errorMessage.value = 'Invalid queries. Please enter only valid integers separated by spaces.';
      results.value = [];
      return;
    }
  
    errorMessage.value = null;
  
    const list = numbersInput.value.split(' ').map(Number).sort((a, b) => a - b);
    const queries = queriesInput.value.split(' ').map(Number);
  
    if (list.some(num => !isValidInteger(num)) || queries.some(num => !isValidInteger(num))) {
      errorMessage.value = 'Please enter only valid integers.';
      results.value = [];
      return;
    }
  
    results.value = findCloseNumber(list, queries);
  };
  </script>
  