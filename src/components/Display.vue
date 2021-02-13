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
            <button @click="deleteGroup()"> <font-awesome-icon :icon="['fas', 'trash']" /> Delete</button>
        </div>
    </div>
</header>
<nav>
    <button class="displaying" id="todos" @click="changeDisplaying('todos')">To Do</button>
    <!-- <button id="notes" @click="changeDisplaying('notes')">Notes</button> -->
</nav>
<main>
    <ToDo v-if="displaying == 'todos'" :note="note" />
</main>
</div>
</template>

<script>
import ToDo from './Display/Todo.vue'

export default {
    components:{
        ToDo
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
        changeDisplaying(displaying){
            this.displaying = displaying

            document.querySelector('.displaying').classList.remove('displaying')
            document.querySelector(`#${this.displaying}`).classList.add('displaying')

            this.checkToDo()
        },

        async deleteGroup(){
           await this.$parent.notes.splice(this.$parent.noteSelected, 1); 
           this.$root.$emit('deleteGroup', this.$parent.noteSelected - 1)
           this.$root.$emit('saveNotes')
        },

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
            background: rgb(80,80,90);
            border: 2px solid rgb(120,120,140);
            width: 512px;
            padding: 0 8px;
            border-radius: 16px;
            font-size: 32px;
            font-weight: bold;
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

        
    }
}
</style>