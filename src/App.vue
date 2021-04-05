<template >
  <div id="app" >
      <h2 class="font-semibold">The Star Wars API</h2>
        <div class="py-12">
            <div class="bg-white shadow-xl sm:rounded-lg flex" style="min-height: 800px; max-height:800px;">
                <div class="w-5/12 bg-gray-200 pl-5 bg-opacity-25 border-r border-gray-200 overflow-y-scroll">
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
                        <li v-if="(searchTerm && peoples.length == 0)"
                            class="p-6 text-lg text-gray-400 leading-7 border-b border-gray-200">
                            <p class="flex items-center">
                                None records found.
                            </p>
                        </li>
                    </ul>
                </div>
                <div class="w-7/12 flex flex-col justify-between">
                    <div class="flex h-auto justify-center">
                        <div class="w-auto p-6 flex flex-col text-2xl">
                            <div v-if="(this.planetActive)"
                                    class="text-left w-auto" >
                                <p>
                                    <b>Home World:</b> <span>{{ this.planetActive.name }}</span>
                                </p>
                                <p>
                                    <b>Diameter:</b> <span>{{ this.planetActive.diameter }}</span>
                                </p>
                                <p>
                                    <b>Gravity:</b> <span>{{ this.planetActive.gravity }}</span>
                                </p>
                                <p>
                                    <b>Orbital Period:</b> <span>{{ this.planetActive.orbital_period }}</span>
                                </p>
                                <p>
                                    <b>Rotation Period:</b> <span>{{ this.planetActive.rotation_period }}</span>
                                </p>
                                <p>
                                    <b>Terrain:</b> <span>{{ this.planetActive.terrain }}</span>
                                </p>
                                <p>
                                    <b>Climate:</b> <span>{{ this.planetActive.climate }}</span>
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div v-if="!(searchTerm)">
                <p class=" pt-2 items-center">
                    <a  v-if="previousPage"
                        class="font-semibold hover:cursor-pointer"
                        @click="() => { loadPeoplePageData(previousPage) }"
                         >
                        Previous 
                    </a>
                    ||
                    <a  v-if="nextPage"
                        class="font-semibold hover:cursor-pointer"
                        @click="() => { loadPeoplePageData(nextPage) }"
                         >
                        Next 
                    </a>
                </p>
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
                planetActive: null,

                previousPage: null,
                nextPage: null,

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
            loadPeoplePageData: async function(pageURL)
            {
                await axios.get(`${pageURL}`).then(response => {
                    this.loadPageResults(response);
                });
            },
            loadPeopleData: async function(peopleURL) 
            {
                this.planetActive = null;

                await axios.get(`${peopleURL}`).then(response => {
                    this.peopleActive = response.data

                    axios.get(`${response.data.homeworld}`).then(response => {
                        this.planetActive = response.data
                    });
                });

            },
            loadInititalData: async function ()
            {
                await axios.get(`${api}/people/`).then( response => {
                    this.loadPageResults(response);
                });
            },
            loadPageResults: function (response)
            {
                this.previousPage = null
                this.nextPage = null
                this.peoples = null

                if(response.data.previous)
                       this.previousPage = response.data.previous
                    
                if(response.data.next)
                    this.nextPage = response.data.next

                this.peoples = response.data.results
            }
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
