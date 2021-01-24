<template>
    <div  style="margin-top:2%">
        <vue-simplemde v-model="body"  />
        <v-btn dark color="#30A026" @click="submit">Reply</v-btn>
    </div>
</template>

<script>
    export default {
        props:['questionSlug'],
        data(){
            return{
                body:null
            }
        },
        methods:{
            submit(){
                axios.post(`/api/question/${this.questionSlug}/reply`,{body:this.body})
                    .then(res => {
                        this.body = ''
                        EventBus.$emit('newReply',res.data.reply)
                        window.scrollTo(0,0)
                    })
            }
        }
    }
</script>

<style>

</style>
