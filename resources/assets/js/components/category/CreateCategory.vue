<template>
    <v-container>
        <v-form @submit.prevent="submit">
            <v-alert v-if="errors" type="error">
                Please give category name
            </v-alert>
            <v-text-field
                v-model="form.name"
                label="Category Name"
                type="text"
                required
            ></v-text-field>
            <v-btn type="submit" color="#E74904" v-if="editSlug" :disabled="disabled">Update</v-btn>
            <v-btn type="submit" color="#469D70" v-else  :disabled="disabled">Create</v-btn>
        </v-form>

        <v-card>
            <v-toolbar color="#6139B6" dark dense>
                <v-toolbar-title>Categories</v-toolbar-title>
            </v-toolbar>

            <v-list>
                <div  v-for="(category,index) in categories" :key="category.id">
                    <v-list-item>

                        <v-list-item-action style="margin-right: 3%">
                            <v-btn icon small @click="edit(index)">
                                <v-icon color="#FF8A28">edit</v-icon>
                            </v-btn>
                        </v-list-item-action>

                        <v-list-item-content>
                            <v-list-item-title>
                                {{category.name}}
                            </v-list-item-title>
                        </v-list-item-content>

                        <v-list-item-action>
                            <v-btn icon small @click="destroy(category.slug)">
                                <v-icon color="#FF0000">delete</v-icon>
                            </v-btn>
                        </v-list-item-action>

                    </v-list-item>
                    <v-divider></v-divider>
                </div>
            </v-list>
        </v-card>
    </v-container>
</template>

<script>
    export default {
        data(){
            return {
                form:{
                    name:null
                },
                categories:{},
                editSlug:null,
                errors:null
            }
        },created() {
            if (!User.admin()){
                this.$router.push('/forum')
            }
            axios.get('/api/category')
                .then(res => this.categories = res.data.data )
        },
        methods:{
            submit(){
                this.editSlug ? this.update() : this.create()
            },
            update(){
                axios.patch(`/api/category/${this.editSlug}`,this.form)
                    .then(res => {
                        this.categories.unshift(res.data)
                        this.form.name = null
                        this.editSlug = null
                    })
            },
            create(){
                axios.post('/api/category',this.form)
                    .then(res => {
                        this.categories.unshift(res.data)
                        this.form.name = null
                    })
                    .catch(error => this.errors = error.response.data.errors)
            },
            destroy(slug,index){
                axios.delete(`/api/category/${slug}`)
                    .then(res => this.categories.splice(index,1))
            },
            edit(index){
                this.form.name = this.categories[index].name
                this.editSlug = this.categories[index].slug
                this.categories.splice(index,1)
            },
        },
        computed:{
            disabled(){
                return !this.form.name
            }
        }

    }
</script>

<style>

</style>
