<template>
    <div class="text-white">
        <img alt="phCollab logo" src="../assets/phCollab_logo.png" class="logo">
        <div class="text-3xl font-bold mt-10 mb-2">TODO</div>
        <div class="grid grid-cols-12 gap-3">
            <div class="col-span-8">
                <input class="h-[40px] w-[100%] text-black px-2" ref="todo_input" type="text"
                    placeholder="Enter your todo here" v-model="input_value" @keyup.enter="addTask()" />
            </div>
            <div class="col-span-4">
                <button class="h-[40px] w-[100%] bg-white text-black" @click="addTask">Add TODO</button>
            </div>
        </div>        
    </div>
</template>

<script>

export default {
    mounted() {
        this.$refs.todo_input.focus()
    },
    data(){
        return{
            input_value: "",  
            task_list: [],          
        }
    },
    methods:{        
        addTask(){      
            const inputVal = this.input_value      
            this.$emit('add', inputVal);
            this.$refs.todo_input.focus()
            this.input_value = "";
        },        
    } 
}
</script>

<style>
body {
    background: #181818 !important;
    overflow: hidden;
}

#app {
    display: flex;
}

.logo {
    border: 2px solid white;

}

.item {
    cursor: pointer;
}
</style>