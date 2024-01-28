<template>
    <ion-page>
        <ion-header>
            <ion-toolbar>
                <ion-title>Ionic Todo App</ion-title>
            </ion-toolbar>
        </ion-header>

        <ion-content class="ion-padding">
            <ion-list>
                <ion-item v-for="task in tasks" :key="task.id">
                    <ion-grid>
                        <ion-row class="ion-align-items-center">
                            <ion-col>{{ task.title }}</ion-col>
                            <ion-col class="ion-text-end">
                                <ion-button
                                    @click="removeTask(task.id)"
                                    color="danger"
                                    fill="clear"
                                >
                                    Remove
                                </ion-button>
                            </ion-col>
                        </ion-row>
                    </ion-grid>
                </ion-item>
            </ion-list>
        </ion-content>

        <div class="bottom-container">
            <ion-input
                @IonChange="newTask = $event.target.value"
                placeholder="Enter a new task"
                @keyup.enter="addTask"
            ></ion-input>
            <ion-button @click="addTask">Add Task</ion-button>
        </div>
    </ion-page>
</template>

<style scoped>
.bottom-container {
    position: fixed;
    bottom: 0;
    width: 100%;
    padding: 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}
</style>

<script setup lang="ts">
import { ref, watch } from 'vue';

const STORAGE_KEY = 'ionic_todo_tasks';

const getTasksFromLocalStorage = () => {
    const storedTasks = localStorage.getItem(STORAGE_KEY);
    return storedTasks ? JSON.parse(storedTasks) : getDefaultTasks();
};

const saveTasksToLocalStorage = (tasks) => {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(tasks));
};

const getDefaultTasks = () => [
    { id: 1, title: 'Default Task 1' },
    { id: 2, title: 'Default Task 2' },
];

const newTask = ref('');
const tasks = ref(getTasksFromLocalStorage());

watch(tasks, (newTasks) => {
    saveTasksToLocalStorage(newTasks);
});

const addTask = () => {
    if (newTask.value.trim() !== '') {
        tasks.value.push({
            id: tasks.value.length + 1,
            title: newTask.value,
        });
        newTask.value = '';
    }
};

const removeTask = (taskId: number) => {
    tasks.value = tasks.value.filter((task) => task.id !== taskId);
};
</script>
