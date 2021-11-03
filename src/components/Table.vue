<script>

  export default {
    data() {
      return {
        searchString: '',
        foodables: [],
      }
    },
    mounted () {
      this.getFoods()
    },
    methods: {
      getFoods () {
        fetch('https://belchertown.stepzen.net/foodable/sheets/__graphql/', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': 'Apikey belchertown::stepzen.net+1000::74fbc99db67e5121aeb21b971dcbbb0fdac153f62715e8434e64b1031604411e',
          },
          body: JSON.stringify({ query: `{ foodable(q: "Select *") { Food__A Price__B } }` }),
        })
        .then(res => this.foodables = res.json())
        .then(res => (this.foodables = res.data.foodable))
        .then(res => (res.filter(v => v.Food__A.toLowerCase().includes(this.searchString.toLowerCase()))))
        .then(res => (this.foodables = res))
        .catch(err => (console.error(err)))
      }
    },
    watch: {
      searchString(newSearchString, oldSearchString) {
        this.getFoods()
      }
    }
  }

</script>

<template>
  <label class="block text-sm relative">
    <input
      class="block w-full mb-6 text-sm dark:border-gray-600 dark:bg-gray-700 focus:border-purple-400 focus:outline-none focus:shadow-outline-purple dark:text-gray-300 dark:focus:shadow-outline-gray form-input"
      placeholder="Find something here..."
      v-model.lazy="searchString"
    />
    <svg xmlns="http://www.w3.org/2000/svg" class="h-full w-6 absolute inset-y-0 right-0 mr-3 text-gray-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
    </svg>
  </label>

  <div class="w-full mb-8 overflow-hidden rounded-lg shadow-xs">
    <div class="w-full overflow-x-auto">
      <table class="w-full whitespace-no-wrap">
        <thead>
          <tr
            class="text-xs font-semibold tracking-wide text-left text-gray-500 uppercase border-b dark:border-gray-700 bg-gray-50 dark:text-gray-400 dark:bg-gray-800">
            <th class="px-4 py-3">Food</th>
            <th class="px-4 py-3">Price</th>
          </tr>
        </thead>
        <tbody class="bg-white divide-y dark:divide-gray-700 dark:bg-gray-800">

          <tr v-for="foodable in foodables" v-bind:key="foodable.id" class="text-gray-700 dark:text-gray-400">
            <td class="px-4 py-3 text-sm">
              {{ foodable["Food__A"] }}
            </td>
            <td class="px-4 py-3 text-sm">
              {{ foodable["Price__B"] }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <div
      class="grid px-4 py-3 text-xs font-semibold tracking-wide text-gray-500 uppercase border-t dark:border-gray-700 bg-gray-50 sm:grid-cols-9 dark:text-gray-400 dark:bg-gray-800">
      <span class="flex items-center col-span-3">
        Showing 21-30 of 100
      </span>
      <span class="col-span-2"></span>
      <!-- Pagination -->
      <span class="flex col-span-4 mt-2 sm:mt-auto sm:justify-end">
        <nav aria-label="Table navigation">
          <ul class="inline-flex items-center">
            <li>
              <button class="px-3 py-1 rounded-md rounded-l-lg focus:outline-none focus:shadow-outline-purple"
                aria-label="Previous">
                <svg aria-hidden="true" class="w-4 h-4 fill-current" viewBox="0 0 20 20">
                  <path
                    d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z"
                    clip-rule="evenodd" fill-rule="evenodd"></path>
                </svg>
              </button>
            </li>
            <li>
              <button class="px-3 py-1 rounded-md focus:outline-none focus:shadow-outline-purple">
                1
              </button>
            </li>
            <li>
              <button class="px-3 py-1 rounded-md focus:outline-none focus:shadow-outline-purple">
                2
              </button>
            </li>
            <li>
              <button
                class="px-3 py-1 text-white transition-colors duration-150 bg-purple-600 border border-r-0 border-purple-600 rounded-md focus:outline-none focus:shadow-outline-purple">
                3
              </button>
            </li>
            <li>
              <button class="px-3 py-1 rounded-md focus:outline-none focus:shadow-outline-purple">
                4
              </button>
            </li>
            <li>
              <span class="px-3 py-1">...</span>
            </li>
            <li>
              <button class="px-3 py-1 rounded-md focus:outline-none focus:shadow-outline-purple">
                8
              </button>
            </li>
            <li>
              <button class="px-3 py-1 rounded-md focus:outline-none focus:shadow-outline-purple">
                9
              </button>
            </li>
            <li>
              <button class="px-3 py-1 rounded-md rounded-r-lg focus:outline-none focus:shadow-outline-purple"
                aria-label="Next">
                <svg class="w-4 h-4 fill-current" aria-hidden="true" viewBox="0 0 20 20">
                  <path
                    d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
                    clip-rule="evenodd" fill-rule="evenodd"></path>
                </svg>
              </button>
            </li>
          </ul>
        </nav>
      </span>
    </div>
  </div>

</template>

<style scoped>
  a {
    color: #42b983;
  }
</style>