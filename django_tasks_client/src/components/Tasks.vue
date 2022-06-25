<template>
    <div class="tasks_container" :class="{tasks_container_night: darkMode}">

        <div class="tasks_header">
            <h1>TODO</h1>
            <img v-if="!darkMode" @click="darkMode = !darkMode" src="../assets/icon-moon.svg">
            <img v-else @click="darkMode = !darkMode" src="../assets/icon-sun.svg">
        </div>

        <div class="add_task" :class="{darkmode: darkMode}">
            <form v-on:submit.prevent="submitForm">
                <div class="form-group">
                    <button type="submit">
                        <div></div>
                    </button>
                </div>
                <div class="form-group input-text">
                    <input type="text" placeholder="Create a new todo..." class="form-control" :class="{darkmode: darkMode}" id="title" v-model="title">
                </div>
            </form>
        </div>

        <div class="tasks_content">
            <ul class="tasks_list" :class="{darkmode: darkMode}">
                <li v-for="task in tasks" :key="task.id">
                    <div class="task_list-flex">
                        <button class="task_list-checkbox" @click="toggleTask(task)">
                            <div v-if="task.completed" style="background-color:rgb(152, 18, 179);border-radius:50%">
                                <img src="../assets/icon-check.svg"/>
                            </div>
                            <div v-else></div>
                        </button>
                        <p v-if="task.completed" class="task_list-title" style="text-decoration:line-through;color:hsl(236, 33%, 92%);">{{ task.title }}</p>
                        <p v-else class="task_list-title" :class="{darkmode: darkMode}">{{ task.title }}</p>
                    </div>
                    <!-- <div>
                        
                    </div> -->
                    <img src="../assets/icon-cross.svg" @click="deleteTask(task)"/>
                </li>
                <div class="tasks_information">
                    <p>{{ tasks.length }} items left</p>
                    <button>Clear Completed</button>
                </div>
            </ul>
        </div>

        <div class="tasks_sort" :class="{darkmode: darkMode}">
            <div class="tasks_sort-flex">
            <p class="active">All</p>
            <p class="">Active</p>
            <p class="">Completed</p>
            </div>
        </div>

    </div>
</template>

<script>

    export default {
        data() {
            return {
                // tasks
                tasks: [''],
                title: '',
                description: '',
                darkMode: false,
            }
        },
        methods: {
            async getData() {
                try {
                    // fetch tasks
                    const response = await this.$http.get('http://localhost:8000/api/tasks/');
                    // set the data returned as tasks
                    this.tasks = response.data; 
                } catch (error) {
                    // log the error
                    console.log(error);
                }
            },
            async submitForm() {
                try {
                    // create a new task
                    const response = await this.$http.post('http://localhost:8000/api/tasks/', {
                        title: this.title,
                        description: this.description,
                        completed: false,
                    });
                    // add the new task to the list of tasks
                    this.tasks.push(response.data);
                    this.title = '';
                    this.description = '';
                } catch (error) {
                    // log the error
                    console.log(error);
                }
            },
            async toggleTask(task) {
                try {
                    // update the task
                    const response = await this.$http.put(`http://localhost:8000/api/tasks/${task.id}/`, {
                        completed: !task.completed,
                        title: task.title,
                        description: task.description,
                    });
                    this.getData(); // Can maybe check to see what they think about this and if there is a better way to do this
                    // Get the index of the task being updated
                    let taskIndex = this.tasks.findIndex(t => t.id === task.id);

                    // Reset the tasks array with the new data of the updated task
                    this.tasks = this.tasks.map((task) => {
                        if (this.tasks.findindex(t=>t.id === task.id) === taskIndex) {
                            return response.data;
                        }
                        return task;
                    });
                } catch (error) {
                    // log the error
                    console.log(error);
                }
            },
            async deleteTask(task) {
                try {
                    // delete the task
                    await this.$http.delete(`http://localhost:8000/api/tasks/${task.id}/`);
                    // remove the task from the list of tasks
                    this.tasks = this.tasks.filter(t => t.id !== task.id);
                } catch (error) {
                    // log the error
                    console.log(error);
                }
            },
        },
        created() {
            // Fetch tasks on page load
            this.getData();
        }
    }
</script>