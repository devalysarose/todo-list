<template>
    <div class="text-white">
        <!-- <TaskList @add="addTask"/> -->
        <div class="border border-white rounded  text-white overflow-auto max-h-80 p-3 mt-3">
            <div class="text-xl text-center" v-show="listEmpty">
                Nothing to display.
            </div>
            <div v-show="!listEmpty">
                <div v-for="(item, index) in task_list" :key="item.key">
                    <div class="grid grid-cols-12 gap-3">
                        <div class="col-span-8">
                            TODO #{{ item.key }}
                        </div>
                        <div class="col-span-4">
                            <button class="float-right h-[20px] mr-2 text-white font-bold"
                                @click="closeTask(item.key)">x</button>
                        </div>
                    </div>

                    <div class="p-2" v-show="!item.edit" @click="enableEdit(item.key)">{{ item.name }}</div>
                    <div v-show="item.edit">
                        <input class="h-[40px] w-[100%] text-black px-2 mt-2 mb-2" :ref="getInputRef(index)"
                            :value="item.name" type="text" @keyup.enter="updateIndexValue(item.key)" />
                        <p class="italic text-xs">Press enter to update</p>
                    </div>
                    <span class="date float-right">{{ item.date }}</span>
                    <br>
                    <hr class="mb-2">
                </div>
            </div>
        </div>
    </div>
   
</template>

<script>
// import TaskList from './InputTask.vue'
export default{
    props: { task_list: Object, listEmpty:Boolean },
    data() {
        return {            
            index: 0,            
            currentDate: null,
            input_ref: "edit-ref"
        }
    },
    methods: {
        getInputRef(index) {
            return this.input_ref + "_" + index
        },
    
        closeTask(key) {
            const index = this.task_list.findIndex(item => item.key === key);

            this.task_list.splice(index, 1);
            const task_len = Object.keys(this.task_list).length;
            
            if (task_len < 1) {
                // this.listEmpty = true
                this.$emit('update:listEmpty', true);
            }
        },

        enableEdit(key) {
            const targetObject = this.task_list.find(item => item.key === key);
            const index = this.task_list.findIndex(item => item.key === key);
            targetObject.edit = true;
            const inputElement = this.$refs[this.getInputRef(index)][0];

            this.$nextTick(() => {
                inputElement.focus();
            });
        },

        updateIndexValue(key) {
            const targetObject = this.task_list.find(item => item.key === key);
            targetObject.name = event.target.value
            targetObject.edit = false;
            // this.$refs.todo_input.focus()
        },
        
    }
}
</script>

<style>
.logo {
    border: 2px solid white;
    /* /* margin: auto; */

}
</style>