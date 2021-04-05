<template >
  <div id="app" >
      <h2 class="font-semibold">The Star Wars</h2>
        <div class="py-12">
            <div class="bg-white shadow-xl sm:rounded-lg flex" style="min-height: 800px; max-height:800px;">
                <div class="w-3/12 bg-gray-200 pl-5 bg-opacity-25 border-r border-gray-200 overflow-y-scroll">
                   <ul>
                       <li>
                            <form v-on:keyup="searchPeoples">
                                <div class="flex rounded-md overflow-hidden border border-gray-300">
                                    <input v-model="searchTerm" type="text" class="flex-1 mx-4 py-2 text-sm focus:outline-none border-gray-300">
                                </div>
                            </form>
                        </li>
                        <li :class="(peopleActive && peopleActive.url == people.url) ? 'bg-gray-300 bg-opacity-50' : ''"
                            v-for="people in peoples" :key="people.url"
                            @click="() => {loadPeopleData(people.url)}"
                            class="p-6 text-lg text-gray-600 leading-7 font-semibold border-b border-gray-200 hover:bg-opacity-50 hover:cursor-pointer hover:bg-gray-400">
                            <p class="flex items-center">
                                {{ people.name }}
                            </p>
                        </li>
                    </ul>
                </div>
                <div class="w-10/12 flex flex-col justify-between">
                    <div class="w-full p-6 flex flex-col overflow-y-scroll h-full">
                       
                    </div>
                </div>
            </div>
      </div>
  </div>
</template>

<script>
    import axios from '../node_modules/axios'
    var api = 'https://swapi.dev/api';
    
    export default {
        name: 'app',
        components: {
        },
        data() {
            return {
                peoples: [],
                peopleActive: null,

                searchTerm: '',
            }
        },
        methods: {
            searchPeoples: async function()
            {
                if(this.searchTerm)
                {
                    await axios.get(`${api}/people/?search=${this.searchTerm}`).then(response => {
                        this.peoples = response.data.results
                    });
                }
                else
                {
                    this.loadInititalData();
                }

            },
            loadInititalData: async function ()
            {
                axios.get(`${api}/people/`).then( response => {
                    console.log(response)
                    this.peoples = response.data.results
                });
            },
        },
        mounted() 
        {
            this.loadInititalData();
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
