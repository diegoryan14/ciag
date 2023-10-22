<template>
    <div class="container texte-center">
        <div class="row">
            <div class="row col-12">
                <div class="container texte-center">
                    <div class="row">
                        <div class="row col-12 col-sm-12 col-md-12 col-lg-12 col-xl-12" style="margin-top: 20px;">
                            <div class='col-12 col-sm-6 col-md-6 col-lg-6 col-xl-6 d-flex flex-column justify-content-center' style="margin-bottom: 20px;">
                                <span>Order:</span>
                                <select class="form-select" aria-label="Ordem" v-model='input.ORDEM' @change="eventOrdem()">
                                <option v-for='x in dataSourceOrdem' :value="x.CODORDEM"> {{x.NOME}} </option>
                                </select>
                            </div>
                            <div class='col-12 col-sm-6 col-md-6 col-lg-6 col-xl-6 d-flex flex-column justify-content-center' style="margin-bottom: 20px;">
                                <span>Filter by Category:</span>
                                <select class="form-select" aria-label="Category" v-model='input.CATEGORY' @change="eventCategory()">
                                <option v-for='x in dataSourceCategory' :value="x"> {{x}} </option>
                                </select>
                            </div>
                        </div>
                        <div class="col-12 col-sm-12 col-md-12 col-lg-12 col-xl-12" style="margin-top: 20px;">
                            <h4 class="text-center" style="font-weight: bold; font-style: italic; font-family: 'Courier New', Courier, monospace;">Products</h4>
                        </div>
                        <div v-for="x in api" style="margin-top: 5%; margin-bottom: 5%;" class="col-6 mt-4 d-flex justify-content-center">
                            <div class="col-sm-12 col-md-8">
                                <div id="card" style="background-color: white; height: 550px;" class="card">
                                    <div  class="card-body row">
                                        <div style="font-weight: bold; font-style: italic; font-family: 'Courier New', Courier, monospace;" class="text-center"><b>{{x.title}}</b></div>
                                        <div style="margin-top: 5%; margin-bottom: 5%;" class="text-center">
                                            <img :src="x.image" width="150">
                                        </div>
                                        <div style="margin-top: -20px; font-weight: bold; font-style: italic; font-family: 'Courier New', Courier, monospace;">
                                            <p class="mt-4"> <b>Category:</b> {{x.category}}</p>
                                        </div>
                                        <div style="margin-top: -20px; font-style: italic; font-weight: bold; font-family: 'Courier New', Courier, monospace;">
                                            <p class="mt-4"> <b>Price: </b>${{x.price}}</p>
                                        </div>
                                        <div class="col-12 text-center" style="margin-bottom: 10px; margin-top: 20px;">
                                            <button @click="more(x)" type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#Modal_More">More</button>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                 <!-- Modal -->
                <div class="modal fade" id="Modal_More" tabindex="-1" aria-labelledby="Modal_MoreLabel" aria-hidden="true">
                    <div class="modal-dialog modal-xl">
                        <div class="modal-content">
                            <div style="background-color:#292929;" class="modal-header">
                                <h1 style="color:white; text-align: center !important;" class="modal-title fs-5" id="Modal_MoreLabel">Product information {{ modal.title }}</h1>
                                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                            </div>
                            <div style="background-color: #141313" class="modal-body">
                                <div class="row">
                                    <div class="col-md-5"></div>
                                    <div></div>
                                    <div style="margin-top: 2%;  color:white; width: 500px;" class="col-12 mt-12">
                                        <div class="col-sm-12 col-md-12">
                                            <div style="margin-top: -7px; font-style: italic; font-family: 'Courier New', Courier, monospace;">
                                                <p class="mt-12"> <b style="color: orange; font-weight: bold;">Description: </b>{{ modal.description }}</p>
                                            </div>
                                            <div style="margin-top: -7px; font-style: italic; font-family: 'Courier New', Courier, monospace;">
                                                <p class="mt-12"> <b style="color: orange; font-weight: bold;">Quantity in stock: </b>{{ modal.count }}</p>
                                            </div>
                                            <div style="margin-top: -7px; font-style: italic; font-family: 'Courier New', Courier, monospace;">
                                                <p class="mt-12"> <b style="color: orange; font-weight: bold;">Product evaluation: </b>{{ modal.rate }}</p>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div style="background-color:#292929;" class="modal-footer">
                                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
  export default{
    data: function () {
        return {
            api: [],
            modal: {
                title: null,
                count: null,
                description: null,
                rate: null
            },
            dataSourceOrdem: [],
            dataSourceCategory: [],
            input: {
                ORDEM: null,
                CATEGORY: null
            },
            // guarda_api: [],
            // ult_valor: 0
        }
    },
    computed: {
    },
    methods: {
        get_api(){
            fetch('https://fakestoreapi.com/products/').then(res=>res.json()).then(json=>{this.api = json});
        },
        // get_api_test(){
        //     fetch('https://fakestoreapi.com/products/').then(res=>res.json()).then(json=>console.log(json));
        // },
        more(x){
            this.modal.title = x.title;
            this.modal.count = x.rating.count;
            this.modal.description = x.description;
            this.modal.rate = x.rating.rate;
        },
        get_ordem(){
            this.dataSourceOrdem = [{CODORDEM: '0', NOME: 'Aleatório'}, {CODORDEM: '1', NOME: 'Menor Preço'}, {CODORDEM: '2', NOME: 'Maior Preço'}];
        },
        eventOrdem(){
            this.input.CATEGORY = null;
            this.input.ORDEM = parseInt(this.input.ORDEM);
            
            if(this.input.ORDEM == 0){
                this.api = [];
                fetch('https://fakestoreapi.com/products/').then(res=>res.json()).then(json=>{this.api = json});
                this.api = this.guarda_api;
                return;
            }
            if(this.input.ORDEM == 1){
                this.api = [];
                fetch('https://fakestoreapi.com/products?sort=desc').then(res=>res.json()).then(json=>{this.api = json});
                return;
            }
            if(this.input.ORDEM == 2){
                this.api = [];
                fetch('https://fakestoreapi.com/products?sort=asc').then(res=>res.json()).then(json=>{this.api = json});
                return;
            }
        },
        get_categories(){
            fetch('https://fakestoreapi.com/products/categories').then(res=>res.json()).then(json=>{this.dataSourceCategory = json});
        },
        eventCategory(){
            this.input.ORDEM = null;

            this.api = [];
            fetch('https://fakestoreapi.com/products/category/' + this.input.CATEGORY.trim()).then(res=>res.json()).then(json=>{this.api = json});
            return;
        },
    },
    mounted: function () {
        this.get_api();
        // this.get_api_test();
        this.get_ordem();
        this.get_categories();
    }
}
</script>

<style scoped>
</style>