<template>
    <div class="text-white">
        <img alt="phCollab logo" src="../assets/phCollab_logo.png" class="logo">
        <div class="text-3xl font-bold mt-10 mb-2">TODO</div>
        <div class="grid grid-cols-12 gap-3">
            <div class="col-span-8">
                <input class="h-[40px] w-[100%] text-black px-2" ref="todo_input" type="text" placeholder="Enter your todo here" v-model="input_value" @keyup.enter="addTask()"/>
            </div>
            <div class="col-span-4">
                <button class="h-[40px] w-[100%] bg-white text-black" @click="addTask">Add TODO</button>
            </div>            
        </div>
        <div class="border border-white rounded  text-white overflow-auto max-h-80 p-3 mt-3"> 
            <div class="text-xl text-center" v-show="listEmpty">
                Nothing to display.
            </div> 
            <div v-show="!listEmpty">           
                <div  v-for="(item, index) in task_list" :key="item.key">
                    <div class="grid grid-cols-12 gap-3">
                        <div class="col-span-8">
                            TODO #{{ item.key }} 
                        </div>
                        <div class="col-span-4">
                            <button class="float-right h-[20px] mr-2 text-white font-bold" @click="closeTask(item.key)">x</button>
                        </div>                        
                    </div>
                
                    <div class="p-2" v-show="!item.edit" @click="enableEdit(item.key)">{{ item.name }}</div>  
                    <div  v-show="item.edit">
                        <input class="h-[40px] w-[100%] text-black px-2 mt-2 mb-2" :ref="getInputRef(index)" :value="item.name" type="text" @keyup.enter="updateIndexValue(item.key)"/> 
                        <p class="italic text-xs">Press enter to update</p> 
                    </div>                                            
                    <span class="date float-right">{{ currentDate }}</span>
                    <br>
                    <hr class="mb-2">
                </div>                    
            </div>
        </div>                
    </div> 
 
</template>

<script>
export default{
    mounted() {        
        this.$refs.todo_input.focus()
        
    },
    data(){
        return{
            input_value:"",
            task_list:[],
            listEmpty: true,
            index:0,
            activeIndex:null,
            currentDate:null,
            input_ref: "edit-ref"
        }
    },
    methods:{
       getInputRef(index){
            return this.input_ref+"_"+index
       },
        addTask(){
            if(this.input_value){
                this.index++
                this.task_list.push({ key: this.index, name: this.input_value, edit: false });                
                this.listEmpty=false
                this.input_value = "";     
                this.currentDate = this.getCurrentDate()
                this.$refs.todo_input.focus()
            }else{
                alert("No task inputted")
            }            
        },
        
        closeTask(key){                        
            const index = this.task_list.findIndex(item => item.key === key);
            
            this.task_list.splice(index, 1);
            let task_len = Object.keys(this.task_list).length;
            if (task_len<1) {
                this.listEmpty = true
            }
            this.$refs.todo_input.focus()           
        },

        enableEdit(key){
            const targetObject = this.task_list.find(item => item.key === key);
            const index = this.task_list.findIndex(item => item.key === key);
            targetObject.edit = true;          
            const inputElement = this.$refs[this.getInputRef(index)][0];

            this.$nextTick(() => {
                inputElement.focus();
            });           
        },

        updateIndexValue(key){           
            const targetObject = this.task_list.find(item => item.key === key);
            targetObject.name = event.target.value
            targetObject.edit = false;
            this.$refs.todo_input.focus()
        },
        getCurrentDate(){
            const date = new Date(); // Get the current date
            const options = {
                year: 'numeric',
                month: 'numeric',
                day: 'numeric',
                hour: 'numeric',
                minute: 'numeric',
                second: 'numeric',
                hour12: true,
            };

            const formattedDate = date.toLocaleString('en-US', options);
            console.log(formattedDate);
            return formattedDate;
        }
    }
}
</script>

<style>
    body{
        background: #181818 !important;       
        overflow: hidden;
    }
    #app {
        display: flex;
    }

    .logo{
        border: 2px solid white;
        /* /* margin: auto; */
       
    }

    .item{
        cursor: pointer;
    }
    

  
</style>