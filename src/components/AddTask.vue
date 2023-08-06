<template>
    <div class="todo">
        <img alt="phCollab logo" src="../assets/phCollab_logo.png" class="logo">
        <h1>TODO</h1>
        <div>
            <input ref="todo_input" type="text" placeholder="Enter your todo here" v-model="input_value" @keyup.enter="addTask()"/>
            <button class="add-task-btn" @click="addTask">Add TODO</button>
        </div>
        <div class="task-list" v-if="listEmpty"> 
            <p class="text-center">Nothing to display.</p>
        </div>
        <div class="task-list overflow" v-else>           
            <div  v-for="(item, index) in task_list" :key="item.key">
                <div>
                    TODO #{{ item.key }} 
                    <button class="close-btn" @click="closeTask(item.key)">x</button>
                </div>
                
                <div class="item" v-show="!item.edit" @click="enableEdit(item.key)">{{ item.name }}</div>  
                <div  v-show="item.edit">
                    <input class="update-input" :ref="getInputRef(index)" :value="item.name" type="text" @keyup.enter="updateIndexValue(item.key)"/> 
                    <p class="italic text-xs">Press enter to update</p> 
                </div>                                            
                <span class="date">{{ currentDate }}</span>
                <br>
                <hr style="border: 1px solid white; ">
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
                console.log(this.task_list)
                this.listEmpty=false
                this.input_value = "";     
                this.currentDate = this.getCurrentDate()
                this.$refs.todo_input.focus()
            }else{
                alert("No task inputted")
            }            
        },
        
        closeTask(key){
            
            let task_len = Object.keys(this.task_list).length;
            const index = this.task_list.findIndex(item => item.key === key);

            this.task_list.splice(index, 1);
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
    div{
        margin-bottom: 15px;
        margin-top: 10px;
    } 

    .logo{
        border: 2px solid white;
        /* /* margin: auto; */
       
    }
    .todo{
        /* border: 2px solid white; */
        width: 100%;
        /* margin: auto; */
        text-align: left;
        color: white;
        padding: 10px;
    }
    .add-task-btn{
        height: 40px;
    }
    .item{
         /* border: 1px solid white; */
        cursor: pointer;
        /* width: 100%; */
    }
    
    input{
        width: 70%;
        /* width: 300px; */
        height: 40px;
        margin-right: 10px;
        padding: 5px;
    }
    .update-input{
        width: 100%;
    }
    button{
        width: 27%;
        /* width: 100px; */
        height: 30px;
        padding: 5px;
        cursor: pointer;
    }
    .task-list{
        border: 1px solid white;
        max-height: 300px;
        margin-top: 10px;
        padding:10px;
        border-radius: .375rem;
        
    }
    .overflow{
        overflow: auto;
    }
    .text-center{
        text-align: center;
    }
    li{
        
        text-decoration: none;
        list-style: none;
        /* text-align: ; */
    }
    .close-btn{
        background: transparent;
        border: 0px;
        width: 10%;
        height: 10px;
        color: white;        
        float: right;
        font-size: 15px;
        /* margin-top: -20px; */
       
    }
    .box{
        border: 1px solid white; 
        width: 100%;
        
    }
    .date{
        float: right;
        font-size: 14px;
        margin-right: 15px;
    }
    hr{
        margin-right: 15px;
    }
    .text-xs {
        font-size: .75rem;
        line-height: 1rem;
    }
    .italic {
        font-style: italic;
    }
</style>