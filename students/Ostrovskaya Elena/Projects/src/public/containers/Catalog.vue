<template>
    <div class="products" id="catalog">
         <item v-for="item of showItem" :key="item.id_product"  :item="item"/>      
         <item :type="'temp'" @createItem = "createNewProduct"/>   
    </div>
</template>

<script>
import item from '../components/Item.vue'
export default {
    components: {item},
    data() {
        return{
            name: "catalog",
            showItem: [],
            items: [],
            url: '/api/catalog',
            //url:'https://raw.githubusercontent.com/Ostrovskaya/js2_1803/master/students/Ostrovskaya%20Elena/Projects/src/public/api/catalog.json '
        }
    },
    methods:{
        setShowItems(search){
            this.showItem = this.items.filter(item => search == "" || item.product_name.toLowerCase().includes(search))
        },
        createNewProduct(item){
            let id = new Date().getTime();
            let obj = Object.assign({id_product: id}, item)
            this.$parent.postData(this.url, obj)
            .then(data => {
                if(data.result === 1){     
                    this.items.push (obj);    
                } 
            })
        }
    },
    mounted(){
        this.$parent.getData(this.url)
        .then(data => {
            this.items = data;
            this.showItem = data;
        })
    }
}
</script>