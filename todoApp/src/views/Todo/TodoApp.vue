<script setup lang="ts">
import { reactive, ref } from "vue";

const myTask = ref<string>('');
let editableTask = ref<string>('');
const taskList: TaskList[] = reactive([])

const addItems = () => {
    if (myTask.value !== "") {
        taskList.push({
            id: (Math.floor(Math.random() * 25) * Math.floor(Math.random() * 29)).toString(),
            taskName: myTask.value,
            isEditable: false
        });
        myTask.value = "";
    }

}

const editItems = (id: string, taskItem?: string) => {
    if (!taskItem) {
        editableTask.value = taskList.find(task => (task.id === id))?.taskName
        taskList.find(task => task.isEditable = (task.id === id))
    } else {
        taskList.find(task => {
            if (task.id === id) {
                task.taskName = taskItem;
                task.isEditable = false;
            }
        })

    }
}




const deleteItems = (id: string) =>
    taskList.filter((each, index) => {
        if (id === each.id) {
            return taskList.splice(index, 1)
        }
    })

const removeAll = () => taskList.length = 0;

type TaskList = {
    id: string;
    taskName: any;
    isEditable: boolean;
}

</script>

<template>
    <div>
        <div>
            <input class="item-input" v-model="myTask" type="text" placeholder="Enter a task..." @keyup.enter="addItems" />
            <button @click="addItems">Add</button>
            <button @click="removeAll">Remove all</button>
        </div>
        <ul class="task-list" v-if="taskList.length">
            <li v-for="task in taskList" :key="task.id">
                <div>
                    <template v-if="task.isEditable">
                        <input v-if="task.isEditable" type="text" v-model="editableTask" />
                        <button @click="editItems(task.id, editableTask)">Done</button>
                    </template>
                    <template v-else>
                        <span>{{ task.taskName }}</span>
                        <button @click="editItems(task.id)">Edit</button>
                    </template>
                    <button @click="deleteItems(task.id)">X</button>
                </div>
            </li>
        </ul>
    </div>
</template>
<style>
.item-input {
    margin-right: 0.5rem;
}

.task-list {
    padding: 1rem;
}
</style>