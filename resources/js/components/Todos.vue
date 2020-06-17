<template>
    <div>
        <h1>Todo Laravel Vue App</h1>
        <form @submit.prevent="addTodo" class="mb-3">
            <div class="form-group input-group mb-3">
                <input type="text" class="form-control col-md-5" placeholder="Title" v-model="todo.title">
                <div class="input-group-append">
                    <button class="btn btn-outline-secondary" type="submit">Save</button>
                </div>
            </div>            
        </form>
        <div>
            <nav aria-label="Page navigation example">
                <ul class="pagination">
                    <li v-bind:class="[{disabled: !pagination.prev_page_url}]" class="page-item"><a class="page-link" href="#" @click="getTodos(pagination.prev_page_url)">Previous</a></li>

                    <li class="page-item disabled"><a class="page-link text-dark" href="#">Page {{ pagination.current_page }} of {{ pagination.last_page }}</a></li>

                    <li v-bind:class="[{disabled: !pagination.next_page_url}]" class="page-item"><a class="page-link" href="#" @click="getTodos(pagination.next_page_url)">Next</a></li>
                </ul>
            </nav>            
        </div>
        <div>
            <table border=1 style="border-collapse: collapse;"> 
                <tbody>
                    <tr>
                        <th>Id</th>
                        <th>Title</th>
                        <th colspan=2>Action</th>
                    </tr>
                    <tr v-for="todo in todos" v-bind:key="todo.id">
                        <td>{{ todo.id }}</td>
                        <td>{{ todo.title }}</td>
                        <td><a href="#" @click="editTodo(todo)">Edit</a></td>
                        <td><a href="#" @click="deleteTodo(todo.id)">Delete</a></td>
                    </tr>
                </tbody>
            </table>            
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                todos: [],
                todo: {
                    id: '',
                    title: '',
                    body: ''
                },
                todo_id: '',
                pagination: {},
                edit: false
            }
        },

        created() {
            this.getTodos();            
        },

        methods: {
            getTodos(page_url) {
                let pg = this;
                page_url = page_url || '/api/todos';
                fetch(page_url)
                    .then(res => res.json())
                    .then(res => {
                        this.todos = res.data;
                        pg.myPagination(res.meta, res.links);
                    })
                    .catch(err => console.log(err));
            },
            myPagination(meta, links) {
                let pagination = {
                    current_page: meta.current_page,
                    last_page: meta.last_page,
                    next_page_url: links.next,
                    prev_page_url: links.prev
                };

                this.pagination = pagination;
            },
            deleteTodo(id) {
                if(confirm('Are you Sure?')) {
                    fetch(`api/todo/${id}`, {
                        method: 'delete'
                    })
                    .then(res => res.json())
                    .then(data => {
                        alert('Todo Deleted');
                        this.getTodos();
                    })
                    .catch(error => console.log(error));
                }
            },
            addTodo() {
                if (this.edit === false) {
                    // Add
                    fetch('api/todo', {
                        method: 'post',
                        body: JSON.stringify(this.todo),
                        headers: {
                            'content-type': 'application/json'
                        }
                    })
                    .then(res => res.json())
                    .then(data => {
                        this.todo.title = '';
                        alert('Todo Added');
                        this.getTodos(); 
                    })
                    .catch(error => console.log(error));
                } else {
                    // Update
                    fetch('api/todo', {
                        method: 'put',
                        body: JSON.stringify(this.todo),
                        headers: {
                            'content-type': 'application/json'
                        }
                    })
                    .then(res => res.json())
                    .then(data => {
                        this.todo.title = '';
                        alert('Todo Updated');
                        this.getTodos(); 
                    })
                    .catch(error => console.log(error));                    
                }
            },
            editTodo (todo) {
                this.edit = true;
                this.todo.id = todo.id;
                this.todo.todo_id = todo.id;
                this.todo.title = todo.title;
            }
        }
    };
</script>