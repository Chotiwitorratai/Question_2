<template>
<div id="app">
    <div class="container">
        <b-card-group deck>

            <b-card title="Search Catagories" header-tag="header" footer-tag="footer">
                <template #header>
                    <h6 class="mb-0">Catagories</h6>
                </template>
                <b-form-input v-model="text" placeholder="Search catagory name"></b-form-input>
            </b-card>
        </b-card-group>
        <div class="table-data">
            <b-table sticky-header striped hover :items="catagories"></b-table>
        </div>
    </div>
</div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'App',
    data() {
        return {
            text: '',
            data: [],

        }
    },
    computed: {
        catagories() {
            let categories = this.data
            let text = this.text.trimLeft()
            if (text) {
                return this.data.filter(each => {
                    return each.Name.toLowerCase().includes(text.toLowerCase())
                })
            } else {
                return categories;
            }
        }
    },
    mounted() {
        this.getData()
    },
    methods: {
        async getData() {
            try {
                let res = await axios.get(
                    "https://api.publicapis.org/categories"
                )
                // console.log(res.data.categories);
                for await (const each of res.data.categories) {
                    this.data.push({
                        Name: each
                    })
                }
            } catch (error) {
                console.log(error);
            }
        }
    },
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

.b-table-sticky-header {
    max-height: 600px !important;
}
</style>
