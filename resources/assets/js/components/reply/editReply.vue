<template>
    <div>
        <vue-simplemde v-model="reply.reply"  />
        <v-card-actions >
            <v-btn icon small @click="update">
                <v-icon color="#30A026">save</v-icon>
            </v-btn>
            <v-btn icon small @click="cancel">
                <v-icon color="#2C333B">cancel</v-icon>
            </v-btn>
        </v-card-actions>
    </div>
</template>

<script>

    export default {
        props:['reply'],
        methods:{
            cancel(reply){
                EventBus.$emit('cancelEditing',reply);
            },
            update(){
                axios.patch(`/api/question/${this.reply.question_slug}/reply/${this.reply.id}`,{body:this.reply.reply})
                    .then(res => this.cancel(this.reply.reply))
            },
        }

    }
</script>

<style>

</style>
