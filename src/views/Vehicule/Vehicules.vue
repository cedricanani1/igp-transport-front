<template >
  

    <!--====== Page Banner Start ======-->
    <section class="page-banner-area bg_cover" @mouseover="loading(1000)" style="background-image: url(/assets/images/page-banner-1.jpg);">
        <div class="container">
            <div class="row">
                <div class="col-md-6">
                    <div class="page-banner">
                        
                        <h2 class="page-title">liste des vehicules</h2>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!--====== Page Banner Ends ======-->
    
    <!--====== Rechercher Box Start ======-->

    <section class="search-box-area">
        <div class="container">
            <div class="search-box-wrapper">
                <div class="search-header d-md-flex align-items-center justify-content-between">
                    <div class="search-title-field d-lg-flex align-items-center">
                        <h3 class="title">Je cherche :</h3>
                        
                    </div>
                </div>
                <form @submit.prevent="search_car">
                    <div class="search-body">                    
                        <div class="search-form-wrapper d-flex flex-wrap align-items-end">
                            <div class="search-field">
                                <div class="row">
                                    <div class="single-field col-lg-3 col-sm-6">
                                        Marque
                                        <div></div>
                                        <select v-model="search.marque_id" class="form-control">
                                            <option :value="0">Choisir une marque</option>
                                            <option v-for="(marque,index) in marques" :key="index" :value="marque.id" > {{ marque.libelle }} </option>                                                                                          
                                        </select>
                                    </div>
                                    <div class="single-field col-lg-3 col-sm-6">
                                             Types
                                            <div></div>
                                            <select v-model="search.type_id" class="form-control">
                                                <option :value="0">Choisir un type</option>
                                                <option v-for="(type,index) in types" :key="index" :value="type.id" > {{ type.libelle }} </option>                 
                                            </select>
                                    </div>
                                    <div class="single-field col-lg-3 col-sm-6">
                                        Prix
                                        <div></div>
                                        <select v-model="price" class="form-control">
                                            <option :value="[0,150000]">Tous les prix</option>
                                            <option :value="[3000,5000]">FCFA 3,000-FCFA 5,000</option>
                                            <option :value="[10000,15000]">FCFA 10,000-FCFA 15,000</option>
                                            <option :value="[20000,25000]">FCFA 20,000-FCFA 25,000</option>
                                            <option :value="[30000,35000]">FCFA 30,000-FCFA 35,000</option>
                                            <option :value="[40000,45000]">FCFA 40,000-FCFA 45,000</option>
                                            <option :value="[50000,60000]">FCFA 50,000-FCFA 60,000</option>
                                            <option :value="[80000,100000]">FCFA 80,000-FCFA 100,000</option>
                                            <option :value="[125000,150000]">FCFA 125,000-FCFA 150,000</option>                               
                                        </select>
                                    </div>
                                   
                                </div>
                            </div>
                            <div class="search-btn">
                                <button class="main-btn d-block">Rechercher</button>
                            </div>
                        </div>                    
                    </div>
                    
                </form>
            </div>
        </div>
    </section>

    <!--====== Rechercher Box Ends ======-->

   

    <!--====== Inventory Start ======-->

    <section class="inventory-area" >
        <div class="container">
            <div class="cars-tab-menu">
                    <ul class="nav" role="tablist">
                        <li><a class="active main-btn" data-bs-toggle="tab" href="#grid" role="tab">Voir tous les véhicules</a></li>
                        <li> &nbsp; </li>
                        <li><a data-bs-toggle="tab" href="#tab2" role="tab" class="main-btn">Voir les résultats de recherche</a></li> 
                    </ul>
            </div>
            <div class="tab-content">
                <div class="tab-pane fade grid show active" id="grid" role="tabpanel">
                        <div class="browse-wrapper" >
                            <div class="row">
                                <div class="col-lg-4 col-md-6" v-for="vehicule in paginatedvehicles" :key="vehicule.id">
                                        <div class="single-car-item-2 mt-50">
                                            <div class="car-image"  v-if="vehicule.images[0]">
                                                <router-link :to="{name : 'Detail', params:{ id: vehicule.id}}">
                                                    <img :src="'https://igp-backend-transport.lce-ci.com/public/'+vehicule.images[0]"> 
                                                </router-link>
                                            </div>                        
                                            <div class="car-content">
                                                <ul class="car-meta">
                                                    <router-link :to="{name : 'Detail', params:{ id: vehicule.id}}"><h4 class="car-title">{{ vehicule.libelle.toUpperCase() }} - <span class="body-type">{{ vehicule.type.libelle }}</span></h4></router-link>
                                                    <br>
                                                    <li><i class="ion-speedometer"></i> {{ vehicule.transmission.toUpperCase() }}</li>
                                                    <li>
                                                        <span class="price">
                                                            <span class="sale-price"> {{ vehicule.price }} FCFA</span>   
                                                        </span>
                                                    </li>
                                                    <li><i class="ion-map"></i> {{ vehicule.mileage }}</li>
                                                </ul>
                                            </div>
                                        </div>
                                </div>
                                
                            </div> 
                        </div>

                        <ul class="pagination" v-if="vehicules.length > 5 || currentPage > 1">
                        
                    
                            <li class="pagination-item" >
                                <button @click="onClickFirstPage" :disabled="isInPreviousPage" class="main-btn">
                                    Précédent
                                </button>
                            </li>
                        
                            <li>&nbsp;&nbsp;&nbsp;&nbsp;</li>
                            <li class="pagination-item" >
                                <button @click="onClickNextPage" :disabled="isInLastPage" class="main-btn">
                                    Suivant
                                </button>
                            </li>
                        
                            
                        </ul>
                </div>
                <div class="tab-pane fade" id="tab2" role="tabpanel">
                        <div class="browse-wrapper" >
                            <div class="row" id="easyPaginate">
                                <div class="col-lg-4 col-md-6" v-for="result in paginatedResults" :key="result.id">
                                        <div class="single-car-item-2 mt-50">
                                            <div class="car-image"  v-if="result.images[0]">
                                                <router-link :to="{name : 'Detail', params:{ id: result.id}}">
                                                    <img :src="'https://igp-backend-transport.lce-ci.com/public/'+result.images[0]" > 
                                                </router-link>
                                            </div>                        
                                            <div class="car-content">
                                                <ul class="car-meta">
                                                    <router-link :to="{name : 'Detail', params:{ id: result.id}}"><h4 class="car-title">{{ result.libelle.toUpperCase() }} - <span class="body-type">{{ result.type.libelle }}</span></h4></router-link>
                                                    <br>
                                                    <li><i class="ion-speedometer"></i> {{ result.transmission.toUpperCase() }}</li>
                                                    <li>
                                                        <span class="price">
                                                            <span class="sale-price"> {{ result.price }} FCFA</span>   
                                                        </span>
                                                    </li>
                                                    <li><i class="ion-map"></i> {{ result.mileage }}</li>
                                                </ul>
                                            </div>
                                        </div>
                                </div>
                                
                            </div> 
                        </div>

                        <ul class="pagination" v-if="results.length > 5 || currentPage > 1">
                        
                    
                            <li class="pagination-item" >
                                <button @click="onClickPreviousPage" :disabled="isInFirstPage" class="main-btn">
                                    Précédent
                                </button>
                            </li>
                        
                            <li>&nbsp;&nbsp;&nbsp;&nbsp;</li>

                            <li class="pagination-item" >
                                <button @click="onClickNextPage" :disabled="isInLastPage" class="main-btn">
                                    Suivant
                                </button>
                            </li>
                        
                            
                        </ul>
                </div>
            </div>
        </div>
    </section>

    <!--====== Inventory Ends ======-->
    
   <!--====== Call To Action Start ======-->

   

    <!--====== Call To Action Ends ======-->

    
</template>
<script>
import axios from 'axios'
//import store from '@/store'
import Swal from 'sweetalert2'
//import $ from "jquery"


export default {
  name: 'Vehicules',
  props:[
        "total",
        "pageChanged"
  ],
    computed:{
        paginatedResults(){
            let start = (this.currentPage * this.perPage) - this.perPage
            let end = start + this.perPage
            return this.results.slice(start, end)
        },
        paginatedvehicles(){
            let start = (this.currentPage * this.perPage) - this.perPage
            let end = start + this.perPage
            return this.vehicules.slice(start, end)
        },
        startPage(){
            if (this.currentPage === 1) return 1
            if (this.currentPage === this.totalPages) return this.totalPages - this.maxVisibleButtons + (this.maxVisibleButtons - 1)
            return this.currentPage - 1
        },
        endPage(){
            return Math.min(this.startPage + this.maxVisibleButtons - 1, this.totalPages)
        },
        pages(){
            let range = []
            for (let i = this.startPage; i <= this.endPage; i++) {
                range.push({
                    number: i,
                    isDisabled : i === this.currentPage
                })
            }
            return range
        },
        isInFirstPage(){
            return this.currentPage === 1
        },
        isInLastPage(){
            return this.currentPage === this.totalPages
        },
    },
components: {
    
},
  data() {
        return {
            vehicules: [],
            imgs: [],
            marques : [],
            types : [],
            strUser: '',
            load: false,
            price:'0,150000',
            search:{
                type_id:0,
                marque_id:0,
                price: [],
            },
            results:[],
            currentPage:1,
            perPage:6,
            totalPages:Math.ceil( this.total / this.perPage)
        }
    },
    methods:{
        onPageChange(page){
            return this.currentPage = page;
        },
        onClickFirstPage(){
            this.onPageChange(1)
        },
        onClickPreviousPage(){
            this.onPageChange(this.currentPage - 1)
        },
        onClickNextPage(){
            this.onPageChange(this.currentPage + 1)
        },
        onClickLastPage(){
            this.onPageChange(this.totalPages)
        },
        onClickPage(page){
            this.onPageChange(page)
        },
        isActivePage(page){
            return this.currentPage === page
        },
        search_car: function (){
            if(this.search.type_id != 0 || this.search.marque_id != 0){
                let app = this
                let tab =[]
                app.search.price = app.price.split(',')
                console.log(app.search)
                axios.post('/searchCar', app.search)
                .then(function (reponse){
                    console.log('rep',reponse.data)
                    reponse.data.forEach(element => {
                        
                        app.imgs = element.photo
                        tab = app.imgs.split(';')
                        element.images = tab 
                        element.images.pop()
                        
                    });
                    app.results = reponse.data 
                    if (app.results.length==0)
                    {
                        Swal.fire('Information',
                            'Aucun résultat correspondant à votre recherche.',
                            'info'
                        )
                    }
                    else
                    {
                        Swal.fire('Information',
                            app.results.length+' résultat(s) correspondant(s) à votre recherche.',
                            'info'
                        )
                    }
                })
                .catch(function (error){
                    Swal.fire('Information',
                            'Une erreur s\'est produite veuillez réessayer.',
                            'error'
                        )
                    console.log('err',error)
                })
            }
            else{
                Swal.fire('Information',
                            'Renseignez TOUS les champs.',
                            'warning'
                        )
            }
        }
    },
    mounted(){
        
        let app = this
        axios.get('/cars')
        .then(function (reponse) {
            console.log('response',reponse.data)
            let tab =[]
            if (reponse.data){
                reponse.data.forEach(element => {
                    
                    app.imgs = element.photo
                    tab = app.imgs.split(';')
                    element.images = tab 
                    element.images.pop()
                    
                });
                
                app.vehicules = reponse.data
                
            } 
        })
        .catch(function (error){
                 console.log('err',error)
                 Swal.fire({title: 'Erreur',
            text:'Echec de récupération des données',
            icon:'error',
            showConfirmButton: false,
            timer:3000
          });
             })

        axios.get('/car-marques' )
        .then(function (reponse) {
            console.log('response',reponse.data)
            app.marques = reponse.data
        })
        .catch(function (error){
                 console.log('err',error)
             })

        axios.get('/car-types' )
        .then(function (reponse) {
            console.log('resp-data',reponse.data)
            app.types = reponse.data
        })
        .catch(function (error){
                 console.log('err',error)
             })
    },
}
</script>