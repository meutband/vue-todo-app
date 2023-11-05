<script>
export default{
    props:{
        todos: Array
    },
    emits:['delete-todo', 'edit-todo', 'toggle-complete'],
}
</script>
<template>
    <div>
        <table class="table">
            <thead>
                <tr>
                    <th scope="col">To Do</th>
                    <th scope="col">Date Created</th>
                    <th scope="col">Complete</th>
                    <th scope="col">Edit</th>
                    <th scope="col">Delete</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="todo in todos" :key="todo.id">
                    <template v-if="!todo.complete">
                        <th scope="row">{{ todo.text }}</th>
                        <td>{{ todo.date }}</td>
                        <td>
                            <button @click="$emit('toggle-complete', todo)"
                                    class="btn btn-success">
                                Complete
                            </button>
                        </td>
                        <td>
                            <button @click="$emit('edit-todo',todo)"
                                    class="btn btn-warning">
                                Edit
                            </button>
                        </td>
                        <td>
                            <button @click="$emit('delete-todo',todo.id)"
                                    class="btn btn-danger">
                                Delete
                            </button>
                        </td>
                    </template>
                </tr>
            </tbody>
        </table>

        <br />

        <h1>Completed Todos</h1>
        <table class="table">
            <thead>
                <tr>
                    <th scope="col">To Do</th>
                    <th scope="col">Date Completed</th>
                    <th scope="col">Complete</th>
                    <th scope="col">Delete</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="todo in todos" :key="todo.id">
                    <template v-if="todo.complete">
                        <th scope="row">{{ todo.text }}</th>
                        <td>{{ todo.dateCompleted }}</td>
                        <td>
                            <button @click="$emit('toggle-complete', todo)"
                                    class="btn btn-success">
                                Uncomplete
                            </button>
                        </td>
                        <td>
                            <button @click="$emit('delete-todo',todo.id)"
                                    class="btn btn-danger">
                                Delete
                            </button>
                        </td>
                    </template>
                </tr>
            </tbody>
        </table>
    </div>
</template>