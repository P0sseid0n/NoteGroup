<template>
<ul>
    <li class="todo" v-for="(todo, index) in note.todos" :key="todo[2]" 
    @mouseenter="observeMouseToDo(index)" @mouseleave="observeMouseToDo(null)"
     draggable="true" @dragstart="dragTodo($event, true)" @dragover.stop @dragend="dragTodo($event, false)" 
     @dragover.prevent @drop.stop @drop.prevent="dropTodo">
        <div>
            <div>
                
                <input type="checkbox" :id="'todo-' + index" :checked='todo[0]'> 
                <label :for="'todo-' + index" @click="todoCheck(index)"><font-awesome-icon :icon="['fas', 'check']" /></label>
            </div>
            <div>
                <input type="text" v-if="editingTodo === index" minlength="2" maxlength="75">
                <h1 v-else @dblclick="editTodo(index)">{{ todo[1] }}</h1>
            </div>
        </div>
        <div v-if="mouseOver == index && editingTodo === false || editingTodo === index">
            <div v-if="editingTodo === index">
                <button @click="editTodo(false)"><span><font-awesome-icon :icon="['fas', 'check']" /></span> <p>Confirm</p></button>
            </div>
            <div v-else>
                <button @click="editTodo(index)"><span><font-awesome-icon :icon="['fas', 'pen']" /></span> <p>Edit</p></button>
                <button @click="deleteTodo(index)"><span><font-awesome-icon :icon="['fas', 'trash']" /></span> <p>Delete</p></button>
            </div>
        </div>
    </li>  
    <li>
        <button id="add-todo" @click="addTodo()"> <span><font-awesome-icon :icon="['fas', 'plus']" /></span> Add to do </button>
    </li>
</ul>
</template>

<script>
export default {
    data(){
        return{
            mouseOver: null,
            editingTodo: false
        }
    },
    props: {
        note: Object
    },
    methods:{
        dragTodo(e, state){
            if(state){
                e.dataTransfer.setData('todo', this.mouseOver)
    
                e.target.style.opacity =  '0.4'
            } else {
                e.target.style.opacity =  '1'
            }
        },
        dropTodo(e){
            const liDrop = e.path.find(el => el.tagName == 'LI')
            const idNew = liDrop.querySelector('input[type=checkbox]').id.replace('todo-', '')
            const idOld = e.dataTransfer.getData('todo')
            const todos = this.$props.note.todos

            const current = todos[idOld]
            const target = todos[idNew]

            todos[idNew] = current
            todos[idOld] = target


            this.$root.$emit('saveNotes')
            
        },
        addTodo(){
            const todos = this.$props.note.todos
            let id = -1

            if(todos.length > 0) id = todos[todos.length - 1][2]

            this.$props.note.todos.push([false, 'New todo', id + 1 ])
            this.$parent.checkToDo()
            this.$root.$emit('saveNotes')
        },

        deleteTodo(index){
            this.$props.note.todos.splice(index, 1)
            this.mouseOver = null
            this.$root.$emit('saveNotes')
        },

        async editTodo(edit){
            if(!isNaN(edit) && edit !== false){
                this.editingTodo = edit
                const note = await this.$props.note.todos[this.editingTodo][1]


                const li = document.querySelectorAll('.todo')[this.editingTodo]
                li.draggable = false

                const input = li.querySelector('input[type=text]')

                input.value = note
                input.focus()
            } else {
                const li = document.querySelectorAll('.todo')[this.editingTodo]
                li.draggable = true

                const input = li.querySelector('input[type=text]')
                
                this.$props.note.todos[this.editingTodo][1] = input.value.slice(0, 75)

                this.editingTodo = false
                this.$root.$emit('saveNotes')
            }
        },

        observeMouseToDo(index){
            if(index == this.mouseOver) return 

            if(this.mouseOver !== null) document.querySelector('.seeAll').classList.remove('seeAll')
            this.mouseOver = index

            if(index == null) return 
    
            document.querySelectorAll('.todo')[index].classList.add('seeAll')
        },

        todoCheck(id){
            this.$props.note.todos[id][0] = !this.$props.note.todos[id][0]
            this.$root.$emit('saveNotes')
            this.$parent.checkToDo()
        }
    },
    mounted(){
        this.$root.$on('selecting-todo', () => this.editingTodo !== false ? this.editTodo(false) : '')

    }
}
</script>

<style lang="scss">
ul{
    list-style-type: none;

    .todo{
        display: flex;
        flex-direction: column;
        background-color: rgba(230,230,250, .075);
        margin-bottom: 16px;
        border-radius: 16px;
        padding: 8px 16px;
        cursor: move;

        > div:first-of-type{
            display: flex;
            align-items: center;
            h1{
                font-size: 18px;
            }

            > div:first-of-type input{
                display: none;
            }

            > div:last-of-type input{
                height: 40px;
                display: block;
                background: rgb(80,80,90);
                border: 2px solid rgb(120,120,140);
                width: 768px;
                padding: 0 8px;
                border-radius: 8px;
                font-size: 18px;
                font-weight: bold;
            }
        }

        > div:last-of-type:not(div:first-of-type) {
            margin-top: 10px;
            button{
                background: transparent;
                outline: 0;
                border: 0;
                padding: 4px 16px;
                cursor: pointer;
                font-weight: 700;

                &:hover{
                    color: var(--blue);
                }

                span, p{
                    color: inherit;
                }

                span{
                    font-size: 18px;
                }
                
                p{
                    opacity: .8;
                }

                &:not(button:last-of-type){
                    border-right: 2px solid rgb(100,100,110);
                }
            }
        }


        input{
            ~ label{
                width: 24px;
                height: 24px;
                border: 3px solid rgb(100,100,100);
                border-radius: 25%;
                display: flex;
                justify-content: center;
                align-items: center;
                font-size: 20px;
                color: transparent;
                transition: 0.3s;
                margin-right: 16px;
                cursor: pointer;
            }
        }

        input:checked{
            ~ label{
                color: white;
                background-color: var(--blue);
                border: 4px solid var(--blue);
            }
        }
    }

    #add-todo{
        background: none;
        border: 0;
        outline: 0;
        width: 100%;
        cursor: pointer;
        text-align: left;
        font-size: 16px;
        border-radius: 8px;
        padding: 4px 16px;
        color: white;
        font-weight: bold;
        text-transform: uppercase;
        border: 2px dashed rgb(120,120,130);
        transition: color .3s;

        &:hover{
            color: var(--blue-light);
        }

        span{
            color: inherit;
            font-size: 24px;
            vertical-align: -2px;
            padding-right: 16px;
        }
    }
}
</style>