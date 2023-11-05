<script>
export default{
    props:{
        editTodo: null
    },
    data(){
        return{
            text: ""
        }
    },
    methods:{
        onSubmit(e){
            if(!this.text){
                alert("Please enter todo text");
            }
            else{
                const editedTodo = {
                    ...this.editTodo,
                    text: this.text
                }
                this.$emit('submit-edit',editedTodo);
                this.text = "";
            }
        }
    },
    created(){
        this.text = this.editTodo.text;
    },
}
</script>
<template>
    <form @submit.prevent="onSubmit">
        <input v-model="text" type="text">
        <button type="submit" class="btn btn-primary">
            Edit Todo
        </button>
    </form>
</template>