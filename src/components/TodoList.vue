<template>
    <div class="text-white">
        <InputTask @add="returnValue"/>
        <LoadTask :task_list="task_list" :listEmpty="listEmpty" @update="updatelistEmpty"/>
    </div>
</template>

<script>
import InputTask from './InputTask.vue';
import LoadTask from './LoadTask.vue';

export default{
    components:{InputTask, LoadTask},
    data(){
        return{
            value:"",
            task_list:[],
            listEmpty:true,
            index: 0,
        }
    },
    methods:{
        returnValue(input){
            this.value = input
            this.listEmpty = false

            if (this.value) {
                const date = new Date();
                const currentDate = date.toLocaleString();
                this.index++                
                
                this.task_list.push({ key: this.index, name: this.value, edit: false, date: currentDate });
                this.listEmpty = false

                console.log(this.task_list)
            } else {
                alert("No task inputted")
            }
        },

        updatelistEmpty(new_value){
            this.listEmpty = new_value
            this.index= 0
        }
    }
    
}

</script>