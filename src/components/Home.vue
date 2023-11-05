<script>
import 'bootstrap/dist/css/bootstrap.min.css'

import ToDoList from './ToDoList.vue'
import AddToDo from './AddToDo.vue'
import EditToDo from './EditToDo.vue'

export default{
	components:{
		ToDoList,
		AddToDo,
		EditToDo
	},
	data(){
		return{
			todos: [],
			editMode: false,
			editTodo: null,
			endPoint: "http://localhost:4000/todos/"
		}
	},
	computed:{
		totalTodos(){
			return this.todos.length;
		},
		pendingTodos(){
			return this.todos.filter((todo) => {return !todo.complete}).length;
		},
		completedTodos(){
			return this.todos.filter((todo) => {return todo.complete}).length;
		}
	},
	methods:{
        async deleteTodo(todoId){
            if(confirm("Are you sure?")){
				const res = await fetch(this.endPoint+todoId, {method: "DELETE"});
				res.status === 200 
					? (this.todos = this.todos.filter((todo) => todo.id !== todoId)) 
					: alert("Error deleting todo");
            };
        },
		async addTodo(todo){
			const res = await fetch(this.endPoint, {
				method: "POST",
				headers: {"Content-Type": "application/json"},
				body: JSON.stringify(todo)
			});
			const data = await res.json();
			this.todos = [...this.todos, data];
		},
		handleEdit(todo){
			this.editMode = true;
			this.editTodo = todo;
		},
		async submitEdit(editedTodo){
			const res = await fetch(this.endPoint + editedTodo.id, {
				method: "PUT",
				headers: {"Content-Type": "application/json"},
				body: JSON.stringify(editedTodo)
			});
			const data = await res.json();
			
			this.todos = this.todos.map(
				(todo) => todo.id === editedTodo.id 
					? {...todo, text:data.text}
					: todo
			);

			this.editMode = false;
			this.editTodo = "";
		},
		async fetchTodos(){
			const res = await fetch(this.endPoint);
			const data = await res.json();
			return data;
		},
		async toggleComplete(todo){

			todo.complete = !todo.complete;
			if (todo.complete){
				todo.dateCompleted =  new Date().toDateString();
			}
			else{
				todo.dateCompleted = false;
			}

			const res = await fetch(this.endPoint + todo.id, {
				method: "PUT",
				headers: {"Content-Type": "application/json"},
				body: JSON.stringify(todo)
			});
			const data = await res.json();
			
			this.todos = this.todos.map(
				(temptodo) => temptodo.id === todo.id 
					? {...temptodo, 
						complete: data.complete, 
						dateCompleted: data.dateCompleted}
					: temptodo
			);
		}
    },
	async created(){
		this.todos = await this.fetchTodos();
	}
}
</script>

<template>
	<EditToDo v-if="editMode" :edit-todo="editTodo" :key="editTodo.id" @submit-edit="submitEdit"/>
	<AddToDo v-else @add-todo="addTodo" />
	<br />
	<h4>Total Todos: {{ totalTodos }} | Pending: {{ pendingTodos }} | Completed: {{ completedTodos }}</h4>
	<br />
	<ToDoList :todos="todos" @delete-todo="deleteTodo" @edit-todo="handleEdit" @toggle-complete="toggleComplete"/>
</template>

<style>
</style>
