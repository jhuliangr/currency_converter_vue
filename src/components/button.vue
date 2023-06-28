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
            }, 4000);
        }
    },
    watch:{
        clicked(oldVal, _){
            const Obj = () =>{
                if(oldVal){
                    return 'Button locked'
                }
                else{
                    return 'Button unlocked'
                }
            }
            this.$emit('click-state-changed', Obj)
            this.reiniciarClick()
        }
    }
}
</script>