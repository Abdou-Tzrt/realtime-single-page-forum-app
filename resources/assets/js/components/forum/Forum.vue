<template>
    <v-container fluid grid-list-md>
        <v-layout row warp>
            <v-flex xs8>
                <question
                v-for="question in questions"
                :key="question.path"
                :data="question"
                ></question>
                <div class="text-center">
                    <v-pagination
                        v-model="meta.current_page"
                        :length="len"
                        @input="changePage"
                    ></v-pagination>
                </div>
            </v-flex>
            <v-flex xs4>
                <app-sidebar></app-sidebar>
            </v-flex>
        </v-layout>
    </v-container>
</template>

<script>
    import question from "./Question"
    import AppSidebar from "./AppSidebar"
    export default {
        data() {
            return {
                questions:{},
                meta:{},
                len:1,

            }
        },
        components:{question,AppSidebar},
        created() {
            this.fetchQuestion()
        },
        methods:{
            fetchQuestion(page){
                let url = page ? `/api/question?page=${page}` : '/api/question'
                axios.get(url)
                    .then(res => {
                        this.questions = res.data.data
                        this.meta = res.data.meta
                            this.len = res.data.meta.total % res.data.meta.per_page === 0 ?
                                res.data.meta.total / res.data.meta.per_page :
                                Math.floor((res.data.meta.total / res.data.meta.per_page) + 1)
                    })
                    .catch(error => console.log(error.response.data))

            },
            changePage(page){
                this.fetchQuestion(page)
            }
        }
    }
</script>

<style>

</style>
