<template>
    <div class="container shadow-lg " id="contenedor">
        <div class="row">
            <div class="col"></div>
            <div class="col">
                <h1 class="mt-5">Currency Converter</h1>
                <div class="mb-3">
                    <label for="from" class="form-label">From</label>
                    <select class="form-select form-select-lg shadow-lg" name="from" id="from" v-model="from" required >
                        <option v-for="Currency in currencies" :key="Currency" :value="Currency.code">
                            {{ Currency.code }}
                        </option>
                    </select>
                </div>
                    <div class="mb-3">
                    <label for="To" class="form-label">To</label>
                    <select class="form-select form-select-lg shadow-lg" name="To" id="To" v-model="to" required>
                        <option v-for="Currency in currencies" :key="Currency" :value="Currency.code">
                            {{ Currency.code }}
                        </option>
                    </select>
                    </div>
                <hr/>
                <div class="mb-3">
                    <label for="amount" class="form-label">Amount</label>
                    <input type="text" class="form-control" name="amount" id="amount" aria-describedby="helpId" placeholder="Amount" v-model="amount"/>
                    <small id="helpId" class="form-text text-muted">
                        The amount you want to check
                    </small>
                </div>
                <h4 class="mb-5" id="res">{{ loading? "Converting..." : res }}</h4>
                <Button :title="loading?'Converting': 'Convert'" :disable="disabled" :convert="convertCurrencies" @click-state-changed="click_state"/>            
            </div>
            <div class="col"></div>
        </div>
    </div>
</template>

<script>
import { toast } from 'vue3-toastify'
import 'vue3-toastify/dist/index.css'
import Button from '@/components/button'
export default {
    setup(){
        toast('Welcome', {
            autoClose: 2000,
            position: 'top-center',
            theme: 'dark',
            closeButton: false
        })
    },
    data() {
        return {
            currencies: {},
            amount: 1,
            from: "",
            to: "",
            res: 0,
            loading: false,
            condition: false
        }
    },
    methods: {
        fetchData() {
        const monedas = localStorage.getItem("currencies")
        if (monedas) {
            this.currencies = JSON.parse(monedas)
            return
        }
        fetch(`https://api.freecurrencyapi.com/v1/currencies?apikey=${process.env.VUE_APP_ACCESS_KEY_CURRENCIES}`)
            .then(async (res) => {
                const resp = await res.json()
                this.currencies = resp.data
                localStorage.setItem("currencies", JSON.stringify(this.currencies))
            })
            .catch((err) => console.log(err))
        },
        convertCurrencies() {
        if (!this.disabled()) {
            this.loading = true
            fetch(`http://api.exchangeratesapi.io/v1/latest?access_key=${process.env.VUE_APP_ACCESS_KEY_EXCHANGE}`)
            .then((res) =>
                res.json()
                .then((response) => {
                    this.loading = false
                    const obj = response.rates
                    const keys = Object.keys(obj)
                    const values = Object.values(obj)
                    const indexFrom = keys.findIndex((res) => res == this.from)
                    const indexTo = keys.findIndex((res) => res == this.to)
                    this.res = (this.amount * (values[indexTo] / values[indexFrom])).toFixed(4)
                    toast.success('Convertion Done', {
                        autoClose: 1000,
                        position: 'top-center',
                        closeButton: false
                    })
                })
                .catch((err) =>{
                    
                    this.loading = false
                    toast.error('The API exchangeratesapi.io has bocked your country, please, use a VPN and try again',{
                        autoClose: 2000,
                        position: 'top-center',
                        closeButton: false,
                        closeOnClick: true
                    })
                    console.log(err)
                })
            )
            .catch((err) => {
                    this.loading = false
                    console.log(err)
                })
        } else {
            toast.error('Please, select the currencies you want to be converted', {
                position: 'top-center',
                closeButton: false,
                autoClose: 2000
            })
            return
        }
        },
        disabled(){
            return this.condition || (this.to == "" || this.from == "")
        },
        click_state(Obj){
            console.log(Obj())
            this.condition = !this.condition
        }

    },
    mounted() {
        this.fetchData()
    },
    components:{
        Button
    }
    
}
</script>

<style scoped>
#contenedor {
  background-color: rgba(36, 211, 74, 0.681);
  border-radius: 8vw;
}
#res{
    color:cornsilk;
    font-size: 5vh;
    background-color: black;
    opacity: 0.8;
    border-radius: 5vh;
}
</style>