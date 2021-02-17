<template>
<div id="notes">
    <Modal v-if="showNote !== false" :note="note.notes[showNote]" @closeNote="showNote = false"/>


    <li class="note" v-for="(note, index) in note.notes" :key="note[2]" @click="showNote = index" 
    @mouseenter="noteHovering = index" @mouseleave="noteHovering = null" >
        <div>
            <h1> {{ note[0] }} </h1>
            <button @click.stop @click="deleteNote(index)" v-if="noteHovering === index"><font-awesome-icon :icon="['fas', 'trash']" /></button>
        </div>
    </li>




    <li id="addNote" @click="addNote">
        <div>
            <h1><font-awesome-icon :icon="['fas', 'plus']" /></h1>
            <h2>ADD NOTE</h2>
        </div>
    </li>
</div>
</template>

<script>
import Modal from './NotesModal.vue'
export default {
    props: {
        note: Object
    },
    components: {
        Modal
    },
    data(){
        return{
            showNote: false,
            noteHovering: null
        }
    },
    methods: {
        addNote(){
            const notes = this.$props.note.notes
            let id = -1

            if(notes.length > 0) id = notes[notes.length - 1][2]

            this.$props.note.notes.push([ 'New note', '', id + 1 ])
            this.$root.$emit('saveNotes')
        },
        deleteNote(index){
            this.$props.note.notes.splice(index, 1)
            this.$root.$emit('saveNotes')
        }
    },
    created(){
        
    }
}
</script>

<style lang="scss">
#notes{
    li{
        background-color: rgba(230,230,250, .15);
        width: 120px;
        height: 180px;
        border-radius: 16px;
        cursor: pointer;
        vertical-align:middle;
        transition: .3s;
        padding: 0 8px;
        display: inline-block;

        button{
            background: var(--blue);
            outline: 0;
            border: 0;
            padding: 4px 8px;
            margin-top: 8px;
            font-size: 20px;
            border-radius: 8px;
            cursor: pointer;
            box-shadow: rgba(0, 0, 0, 0.15) 1.95px 1.95px 2.6px;

            &:hover{
                background: var(--blue-light);
            }

            &:active{
                background: var(--blue-dark);
            }
        }


        &#addNote{
            background: transparent;
            border: 4px dashed rgb(100,100,120);
            transition: .3s;
        

            h1{
                font-size: 32px;
            }

            h1, h2{
                transition: .5s;
            }

            &:hover {
                border: 4px dashed rgb(150,150,170);

                h1, h2{
                    color: var(--blue);
                }
            }
        }

        &:not(#addNote){            
            &:hover{
                transform: scale(1.05);
                background-color: rgba(230,230,250, .2);
            }
        }

        > div{
            width: 100%;
            height: 100%;
            text-align: center;
            display: flex;
            align-items: center;
            flex-direction: column;
            justify-content: center;
        }

        &:not(li:last-of-type){
            margin-right: 16px;
        }


        h1, h2{
            font-size: 20px;
        }

        
    }
}
</style>