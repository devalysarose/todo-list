<template>
    <div class="todo">
        <img alt="phCollab logo" src="../assets/phCollab_logo.png" class="logo">
        <h3>TODO</h3>
        <div>
            <input type="text" placeholder="Add Task" v-model="input_value"/>
            <button class="btn-success" @click="addTask">Add TODO</button>
        </div>
        <div class="task-list" v-if="listEmpty"> 
            <p class="text-center" >Nothing to display</p>
        </div>
        <div class="task-list overflow" v-if="!listEmpty">
            <ul v-if="!listEmpty">
                <div class="task-header" >                    
                    <li  v-for="(item, objKey, index) in task_list" :key="item.id">
                        <span >{{ objKey }} </span> <br> <br>
                        <span v-if="activeIndex!=index" @dblclick="setActiveIndex(index)">{{ item }}</span>  
                        <input style="width: 90%;" v-else :value="item" type="text" @blur="updateIndexValue(objKey)"/>                      
                        <button class="btn-success close-btn" @click="closeTask(objKey)">x</button>
                        <br>
                        <span class="date">{{ currentDate }}</span>
                        <br>
                        <hr style="border: 1px solid white; ">
                    </li>
                    
                </div>                                
            </ul>                
        </div>        
    </div>    
</template>

<script>
export default{
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
                this.task_list["Task"+ this.index] = this.input_value
                console.log(this.task_list)
                this.listEmpty=false
                this.input_value = "";     
                this.currentDate = this.getCurrentDate()
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
            
        },
        setActiveIndex(index){            
            this.activeIndex=index            
        },
        updateIndexValue(key){
            this.activeIndex=null            
            this.task_list[key] = event.target.value
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
    .todo{
        /* border: 2px solid white; */
        width: 100%;
        /* margin: auto; */
        text-align: left;
        color: white;
        padding: 10px;
    }
    input{
        width: 70%;
        /* width: 300px; */
        height: 25px;
        margin-right: 10px;
        padding: 5px;
    }
    button{
        width: 27%;
        /* width: 100px; */
        height: 30px;
        padding: 5px;
    }
    .task-list{
        border: 1px solid white;
        max-height: 300px;
        margin-top: 10px;
        padding-left:0;
        
    }
    .overflow{
        overflow-y: scroll;
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
        margin-top: -20px;
        /* 0px 10px -50px 5px; */
    }
    .task-header{
        /* border: 1px solid white; */
        display: inline-block;
        height: 20px; 
        width: 100% !important;
        margin-top: 20px;
    }
    .box{
        border: 1px solid white; 
        width: 100%;
    }
    .date{
        float: right;
        font-size: 12px;
        margin-right: 15px;
    }
    hr{
        margin-right: 15px;
    }
</style>