<template>
    <section>
        <div :class="classStyle" class="bg-pad"></div>
        <div class="flex justify-center align-center hero"  :class="(display == true) ? 'block' : 'none'">
            <div class="bg-white br-radius-10 shadow flex p-40 card overflow padder">
                <img :src="apiData.image" alt="product image" class="imgProduct p-50"
                >
                <div class="p-50 flex padder justify-between col">
                    <div >
                        <h3 :class="(classStyle == 'bg-woman') ? 'purple fw-600 fs-28' : 'darkblue fw-600 fs-28'">{{ apiData.title }}</h3>
                        <div class="flex justify-between padder" style="margin-top:30px;">
                            <p class="fs-18 softblack" >{{ apiData.category }}</p>
                            
                            <div class="flex" >
                                <span class="fs-18 softblack"> {{ apiData.rating.rate }}/{{ apiData.rating.count }} </span>
                                <span class="fs-18 softblack"> </span>
                                <div class="flex row">
                                <div v-for="i in 5" :key="i" :class="(classStyle == 'bg-woman') ? (i<=3) ? 'dot bg-purple br-purple' : 'dot br-purple' : (i<=3) ?  ' br-darkblue dot bg-darkblue' : 'dot br-darkblue' "></div>
                            </div>
                            </div>
                        </div>
                        <hr />
                        <p class="fs-20 darkblack">
                            {{ apiData.description }}
                        </p>
                    </div>
                    <div>
                        <hr />
                        <p :class="(classStyle == 'bg-woman') ? 'purple fw-600 fs-28' : 'fw-600 fs-28 darkblue'">${{ apiData.price }}</p>
                        <div class="flex justify-between padder" style="gap: 24px;margin-top: 12px;">
                            <button :class="(classStyle == 'bg-woman') ? 'btn bg-purple br-purple white' : 'btn bg-darkblue br-darkblue white'">Buy Now</button>
                            <button :class="(classStyle == 'bg-woman') ? 'btn-thin bg-white br-purple purple' : 'btn-thin bg-white br-darkblue darkblue'" v-on:click="nextPage">Next Product</button>
                        </div>
                    </div>

                </div>
            </div>
        </div>
        <div class="loaderPad" :class="(display != true) ? 'block' : 'none'">
            <div class="loader"></div> 
        </div>
    </section>
</template>

<script>
import axios from 'axios'
export default {
    name: 'ProductDisplay',
    props: {
        name
    },
    data() {
        return {
            classStyle: 'bg-men',
            page: 1,
            apiData: {},
            category : 'bg-men',
            display: true
        }
    },
    methods: {
        fetchData() {
            axios.get(`https://fakestoreapi.com/products/${this.page}`)
                .then(res => {
                    this.apiData = res.data
                    if(res.data.category == "women's clothing" || res.data.category == "jewelery") {
                        this.classStyle = 'bg-woman'
                    }else{
                        this.classStyle = 'bg-men'
                    }
                    this.display = true
                })
                .catch(err => {
                    alert('error ', err)
                })
        },
        nextPage() {
            if(this.page <20) {
                this.page++
            }else{
                this.page = 1
            }
            this.display = false
            this.fetchData()
        }
    },
    mounted() {
        this.display = false
        this.fetchData()
    }
}
</script>