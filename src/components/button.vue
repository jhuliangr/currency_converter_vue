<template>
    <button class="btn btn-dark mb-3 px-5 py-3 shadow-lg" @click="Convert()" :disabled="disabled()" >
        {{title}}
    </button>
</template>

<script>
export default {
    data(){
        return {
            clicked: false
        }
    },
    props:{
        title:{
            type: String
        },
        convert: {
            type: Function
        },
        disable:{
            type: Function
        }
    },
    methods:{
        Convert(){
            this.clicked = true
            this.convert()
        },
        disabled(){
            return this.disable()
        },
        reiniciarClick(){
            setTimeout(() => {
                this.clicked = false 
            }, 2000);
        }
    },
    watch:{
        clicked(oldVal, newVal){
            this.$emit('click-state-changed')
            this.reiniciarClick()
        }
    }
}
</script>