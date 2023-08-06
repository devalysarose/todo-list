<template>
    <div class="todo">
        <img alt="phCollab logo" src="../assets/phCollab_logo.png" class="logo">
        <h1>TODO</h1>
        <div>
            <input ref="todo_input" type="text" placeholder="Enter your todo here" v-model="input_value" @keyup.enter="addTask()"/>
            <button class="add-task-btn" @click="addTask">Add TODO</button>
        </div>
        <div class="task-list" v-if="listEmpty"> 
            <p class="text-center" >Nothing to display</p>
        </div>
        <div class="task-list overflow" v-if="!listEmpty">
            <div v-if="!listEmpty">
                <div >                    
                    <div  v-for="(item, objKey, index) in task_list" :key="item.id">
                        <div>
                            TODO #{{ objKey }} 
                            <button class="close-btn" @click="closeTask(objKey)">x</button>
                        </div>
                        
                        <div class="item" v-if="activeIndex!=index" @click="setActiveIndex(index)">{{ item }}</div>  
                        <div v-else >
                            <input class="update-input" ref="todo_update_input" :value="item" type="text" @keyup.enter="updateIndexValue(objKey)"/> 
                            <p class="italic text-xs">Press enter to update</p> 
                        </div>                                            
                        <span class="date">{{ currentDate }}</span>
                        <br>
                        <hr style="border: 1px solid white; ">
                    </div>                    
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
            task_list:{},
            listEmpty: true,
            index:0,
            activeIndex:null,
            currentDate:null
        }
    },
    methods:{
        addTask(){
            if(this.input_value){
                // let task_len = Object.entries(this.task_list).length+1
                this.index++
                // this.task_list.push(this.input_value)
                this.task_list[this.index] = this.input_value
                console.log(this.task_list)
                this.listEmpty=false
                this.input_value = "";     
                this.currentDate = this.getCurrentDate()
                this.$refs.todo_input.focus()
            }else{
                alert("No task inputted")
            }            
        },
        
        closeTask(item){
            // this.task_list.pop(item)
            delete this.task_list[item]
            let task_len = Object.entries(this.task_list).length
            
            if (task_len<1) {
                this.listEmpty = true
            }
            this.$refs.todo_input.focus()
            this.activeIndex=null
        },
        setActiveIndex(index){            
            this.activeIndex=index     
            this.$refs.todo_update_input.focus()       
        },
        updateIndexValue(key){
            this.activeIndex=null            
            this.task_list[key] = event.target.value
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