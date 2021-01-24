<template>
    <div v-if="question">
        <edit-question
            :data = question
            v-if="editing">
        </edit-question>

        <show-question
            v-else
            :data = question
        ></show-question>

        <v-container>
            <replies :question="question"></replies>

            <new-reply v-if="loggedIn" :questionSlug="question.slug"></new-reply>
            <div v-else class="mt-4">
                <router-link to="/login">login in to Reply</router-link>
            </div>
        </v-container>

    </div>
</template>

<script>
    import ShowQuestion from './ShowQuestion'
    import EditQuestion from './EditQuestion'
    import Replies from '../reply/replies'
    import NewReply from '../reply/newReply'
    export default {
        components:{ShowQuestion,EditQuestion,Replies,NewReply},
        data(){
            return {
                question:null,
                editing:false,
                beforeEditQuestion:''
            }
        },
        computed:{
            loggedIn(){
                return User.loggedIn()
            }
        },
        created() {
            this.listen()
            this.getQuestion()
        },
        methods:{
            listen(){
                EventBus.$on('startEditing',()=>{
                    this.editing = true
                    this.beforeEditQuestion = this.question.body
                })

                EventBus.$on('cancelEditing',(data)=>{
                    this.editing = false

                    if(data !== this.question.body){
                        this.question.body = this.beforeEditQuestion
                    }
                })
            },
            getQuestion(){
                axios.get(`/api/question/${this.$route.params.slug}`)
                    .then(res => this.question = res.data.data)
            }
        }

    }
</script>

<style>

</style>
