<template>
    

    <!--====== Inventory Single Start ======-->

    <section class="inventory-single-area">
        
        <div class="container">
            <div class="inventory-single-content ">
                
                <div class="title-price">
                    <div class="title-excerpt">
                        <h3 class="entry-title"> Commande : {{ com.order_number }} </h3>
                    </div>
                    
                    <div class="price">
                        <span class="price">
                            <span class="sale-price"> Coût total : {{ com.total_amount }} FCFA</span><br>
                            <span class="sale-price" v-if="com.payment_status=='unpaid'"> Statut :  Non-réglé</span>
                            <span class="sale-price" v-else> Statut :  Réglé</span>
                        </span>
                    </div>
                    
                </div>
                <div class="row justify-content-between">
                    <div class="col-lg-14" v-for="(el,index) in cart" :key="index">
                        
                        <div class="specifications">
                            <h5 class="singe-title">vehicule : {{ el.car.libelle }} </h5>

                            <div class="specifications-wrapper">
                                <div class="row">
                                    <div class="col-md-3 col-sm-4 col-6 glance-col">
                                        <span class="glance">
                                             
                                            <span class="label">prix</span>
                                            <span class="value"> {{ el.price }} </span>
                                        </span>
                                    </div>
                                    
                                    <div class="col-md-3 col-sm-4 col-6 glance-col">
                                        <span class="glance">
                                            
                                            <span class="label">chauffeur</span>
                                            <span class="value" v-if="el.driver==1"> Avec </span>
                                            <span class="value" v-else> Sans </span>
                                        </span>
                                    </div>
                                    <div class="col-md-3 col-sm-4 col-6 glance-col">
                                        <span class="glance">
                                            
                                            <span class="label">période</span>
                                            <span class="value"> Du {{ el.realfrom }} au {{ el.realto }} ({{ el.days }} jours) </span>
                                        </span>
                                    </div>
                                    <div class="col-md-3 col-sm-4 col-6 glance-col">
                                        <span class="glance">
                                            <img :src="'https://igp-backend-transport.lce-ci.com/public/'+ img[index]" :alt="el.car.libelle">
                                        </span>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="specifications">
                            <h5 class="singe-title"> Détails du client </h5>

                            <div class="specifications-wrapper">
                                <div class="row">
                                    <div class="col-md-3 col-sm-4 col-6 glance-col">
                                        <span class="glance">
                                             
                                            <span class="label">nom</span>
                                            <span class="value"> {{ com.nom }} </span>
                                        </span>
                                    </div>
                                    
                                    <div class="col-md-3 col-sm-4 col-6 glance-col">
                                        <span class="glance">
                                            
                                            <span class="label">prenoms</span>
                                            <span class="value"> {{ com.prenoms }} </span>
                                        </span>
                                    </div>
                                    <div class="col-md-6 col-sm-4 col-6 glance-col">
                                        <span class="glance">
                                            
                                            <span class="label">e-mail</span>
                                            <span class="value"> {{ com.email }} </span>
                                        </span>
                                    </div>
                                    <div class="col-md-3 col-sm-4 col-6 glance-col">
                                        <span class="glance">
                                            
                                            <span class="label">contact</span>
                                            <span class="value"> {{ com.phone }} </span>
                                        </span>
                                    </div>
                                    <div class="col-md-3 col-sm-4 col-6 glance-col">
                                        <span class="glance">
                                            
                                            <span class="label">ville</span>
                                            <span class="value"> {{ com.location }} </span>
                                        </span>
                                    </div>
                                    <div class="col-md-6 col-sm-4 col-6 glance-col">
                                        <span class="glance">
                                            
                                            <span class="label">destination</span>
                                            <span class="value"> {{ com.rent_location }} </span>
                                        </span>
                                    </div>
                                </div>
                            </div>
                        </div>
            </div>
        </div>
    </section>  
</template>
<script>
import axios from 'axios'
//import store from '@/store'
import Swal from 'sweetalert2'
import moment from 'moment';
moment.locale('fr');


export default {
    data(){
        return{
            com:[],
            cart:[],
            img:[],
        }
    },
    mounted(){
        let app=this
        let tab = []
        axios.get('/orders/'+this.$route.params.id)
        .then(function (reponse){
            app.com = reponse.data.data
            app.cart = reponse.data.data.cart
            console.log('com',app.com)

            app.cart.forEach(element => {
                        tab = element.car.photo.split(';')
                        element.images = tab 
                        element.images.pop()
                        app.img.push(element.images)
                        element.realfrom = moment(element.from).format("Do/MM/YYYY")
                        element.realto = moment(element.to).format("Do/MM/YYYY")
                    })
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
    }
}
</script>