<template>
    <div>
        <navbar></navbar>
        <div class="container">
        <div class="row justify-content-center" style="margin-top: 3vh;">
            <notes class="col-md" :data="backlog" :cat="`Back Log`"></notes>
            <notes class="col-md" :data="todo" :cat="`To-Do`"></notes>
            <notes class="col-md" :data="doing" :cat="`Doing`"></notes>
            <notes class="col-md" :data="done"  :cat="`Done`"></notes>
        </div>
        </div>
    </div>
</template>

<script>
import db from '../config/firebase'
import navbar from '../components/navbar'
import notes from '../components/notes'
export default {
    name: 'mainpage',
    components:{
        navbar,
        notes
    },
    data(){
        return{
            backlog: [],
            doing:[],
            done: [],
            todo: []
        }
    },
    computed:{
        filter(){
            let list = []
            for(let todo in this.todolist){
                if(todo.status==='backlog'){
                    list.push(todo)
                }
            }
            return list
        },

    },
    created(){
        
        db.collection('kanban').onSnapshot((doc) => {
            let listDoing = []
            let listBacklog = []
            let listDone = []
            let listTodo = []
            doc.docs.forEach(arr=>{
                let todoId = arr.id
                // console.log(arr.id)
                let todo = arr.data()
                if(todo.status=='doing'){
                    listDoing.push({
                        id: todoId,
                        ...todo})
                }else if(todo.status == 'backlog'){
                    listBacklog.push({
                        id: todoId,
                        ...todo})
                }else if(todo.status=='done'){
                    listDone.push({
                        id: todoId,
                        ...todo})
                }else if(todo.status=='todo'){
                    listTodo.push({
                        id: todoId,
                        ...todo})
                }
            })
            this.backlog = listBacklog
            this.doing = listDoing
            this.done = listDone
            this.todo = listTodo
        });
        
    }
}
</script>

<style>

</style>