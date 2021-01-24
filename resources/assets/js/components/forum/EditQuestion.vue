<template>
    <v-container fluid>
        <v-card>
            <v-form @submit.prevent="update">
                <v-text-field
                    v-model="form.title"
                    label="Title"
                    type="text"
                    required
                ></v-text-field>

                <vue-simplemde v-model="form.body"  />

                <v-card-actions>
                    <v-btn icon small type="submit">
                        <v-icon color="#1C9F07">save</v-icon>
                    </v-btn>
                    <v-btn icon small @click="cancel">
                        <v-icon>cancel</v-icon>
                    </v-btn>
                </v-card-actions>

            </v-form>
        </v-card>
    </v-container>
</template>

<script>
    export default {
        props:['data'],
        data(){
            return {
                form: {
                    title : null,
                    body : null
                }
            }
        },
        methods:{
            cancel(data){
                EventBus.$emit('cancelEditing',data)
            },
            update(){
                axios.patch(`/api/question/${this.form.slug}`, this.form)
                    .then(res => this.cancel(this.form.body))
            }
        },
        created(){
            this.form = this.data
        }
    }
</script>

<style>

</style>
