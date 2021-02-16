<template>
<div id="display">
<header>
    <div>
        <input v-if="editingGroup" @keypress.enter="editingGroup = false" type="text" v-model="noteTitle" maxlength="31" minlength="1" autofocus>
        <h1 v-else @dblclick="editingGroup = true"> {{ note.title }} </h1>
    </div>
    <div>
        <button v-if="editingGroup" @click="editingGroup = false"><font-awesome-icon :icon="['fas', 'check']"/> Confirm</button>
        <div v-else>
            <button  @click="editingGroup = true"> <font-awesome-icon :icon="['fas', 'pen']"/> Edit</button>
            <button @click="$root.$emit('deleteGroup')"> <font-awesome-icon :icon="['fas', 'trash']" /> Delete</button>
        </div>
    </div>
</header>
<nav>
    <button :class="{'displaying':  displaying == 'todos'}" id="todos" @click="displaying = 'todos'; checkToDo()">To Do</button>
    <button :class="{'displaying':  displaying == 'notes'}" id="notes" @click="displaying = 'notes'">Notes</button>
</nav>
<main>
    <ul>
        <ToDo v-if="displaying == 'todos'" :note="note" />
        <Notes v-if="displaying == 'notes'" :note="note" />
    </ul>
</main>
</div>
</template>

<script>
import ToDo from './Display/Todo.vue'
import Notes from './Display/Notes.vue'

export default {
    components:{
        ToDo,
        Notes
    },
    props: {
        note: Object
    }, 
    data(){
        return{
            displaying: 'todos',
            editingGroup: false
        }
    },
    methods: {
        checkToDo(){
            setTimeout(() => {
                const note = this.$parent.notes[this.$parent.noteSelected]

                let done = true
                
                note.todos.forEach(todo => {
                    if(!todo[0]) done = false
                })
    
                note.done = done

                
            }, 1)
        }
    },
    mounted(){
        this.$root.$on('selecting-title', () => this.editingGroup = false)
    }, 
    computed: {
        noteTitle: {
            get(){
                return this.$props.note.title
            },
            set(text){
                if(text.length <= 31 && text.length >= 2) this.$props.note.title = text
                this.$root.$emit('saveNotes')
            }
        }
    }
}
</script>

<style lang="scss">
#display{
    background: var(--depth-2);
    height: 100%;
    padding: 8px 24px;

    header{
        display: flex;
        align-items: center;
        justify-content: space-between;
        height: 64px;

        h1{
            font-size: 40px;
        }

        input{
            height: 56px;
            display: block;
            background: var(--depth-4);
            border: 3px solid rgb(80,80,100);
            width: 512px;
            padding: 0 8px;
            border-radius: 16px;
            font-size: 36px;
            font-weight: bold;
            outline: 0;

            &:focus{
                border: 3px solid rgb(120,120,140);
            }
        }

        button{
            background-color: var(--blue);
            border: 0;
            outline: 0;
            font-weight: 700;
            font-size: 15px;
            padding: 8px 8px;
            border-radius: 8px;
            cursor: pointer;
            margin-right: 16px;
            box-shadow: rgba(17, 17, 26, 0.1) 0px 8px 24px, rgba(17, 17, 26, 0.1) 0px 16px 56px, rgba(17, 17, 26, 0.1) 0px 24px 80px;

            &:hover{
                transition: .2s;
                background-color: var(--blue-light);
            }

            &:active{
                transition: 0s;
                background-color: var(--blue-dark);
            }
        }
    }

    nav{
        margin-top: 16px;
        border-bottom: 2px solid rgba(180,180,200,0.3);
        height: 48px;

        button{
            background: transparent;
            opacity: .82;
            border: 0;
            outline: 0;
            cursor: pointer;
            margin-right: 16px;
            font-size: 20px;
            padding: 8px 4px;
            font-weight: 600;
            position: relative;
            top: 1px;
            transition: border .2s;

            &.displaying{
                opacity: .97;
                font-weight: bold;
                border-bottom: 4px solid var(--blue-light);
            }
        }
    }

    main{
        padding-top: 16px;
        height: calc(100% - (64px + 64px));
        overflow-y: auto;

        ul{
            list-style-type: none;
            padding: 0 8px;
        }
    }
}
</style>