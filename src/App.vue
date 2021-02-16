<template>
    <div id="app">
        <header>
            <h1>NoteGroup</h1>
            <h4>Made by <a href="https://posseidon.netlify.app/" target="_blank" rel="noopener noreferrer">P0sseid0n</a></h4>
        </header>
        <main>
            <List :notes="notes" />
            <Display :note="notes[noteSelected]" v-if="notes.length > 0" />
            <NoDisplay v-else />
        </main> 
    </div>
</template>

<script>
import List from './components/List.vue'
import Display from './components/Display.vue'
import NoDisplay from './components/NoDisplay.vue'

export default {
    name: 'App',
    data(){
        return {
            notes: [ 
                { 
                    done: false, 
                    title: 'First note', 
                    todos: [ [false ,'Mouse over here to see options'] ], 
                    notes: [ ['Click to edit this note', "Type the note content" ] ] 
                }, 
            ],
            noteSelected: 0
        }
    },
    components: {
        List, 
        Display,
        NoDisplay
    },
    created(){
        if(localStorage.getItem('notes')) this.notes = JSON.parse(localStorage.getItem('notes'))

        this.notes.forEach((note, index) => {
            note.id = index

            if(!String(note.title.trim())) note.title = 'New Group - ' + index

            let done = true
            note.todos.forEach((todo, index) => {
                if(!todo) {
                    note.todos.splice(index, 1)
                    this.$root.$emit('saveNotes')
                }
                todo[2] = index

                if(!String(todo[1].trim())) todo[1] = 'New todo'

                if(!todo[0]) done = false
            })
            note.notes.forEach((note, index) => {
                note[2] = index
            })

            note.done = done
        })
    },
    mounted(){
        this.$root.$on('saveNotes', () => {
            localStorage.setItem('notes', JSON.stringify(this.notes))
        })
    }
}
</script>

<style lang="scss">
:root{
    --groups: rgb(40,40,50);
    --groups-hovered: rgb(45,45,55);

    --blue-light: rgb(0, 190, 255);
    --blue: rgb(0, 160, 255);
    --blue-dark: rgb(0, 130, 255);

    --depth-1: rgb(30,30,40);
    --depth-2: rgb(40,40,50);
    --depth-3: rgb(50,50,60);
    --depth-4: rgb(60,60,70);
}

::-webkit-scrollbar {
    width: 8px;
}
::-webkit-scrollbar-track {
    background: transparent; 
}

::-webkit-scrollbar-thumb {
    background: rgb(75,75,85); 
    border-radius: 32px;
    transition: .3s;
}

::-webkit-scrollbar-thumb:hover {
    background: rgb(85,85,95); 
}

*{
    font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    color: white;
}

html,body, #app{
    height: 100%;
    background-color: var(--depth-1);
}

#app{
    > header{
        background-color: var(--depth-3);
        padding: 0 4%;
        display: flex;
        justify-content: space-between;
        align-items: center;
        height: 72px;
        
        h1{
            font-size: 36px;
        }

        h4{
            font-style: italic;

            a{
                color: var(--blue);
                text-decoration: none;
                transition: .2s;

                &:hover{
                    color: var(--blue-light);
                    text-decoration: underline;
                }
            }
        }
    }

    > main{
        height: calc(100% - 72px);
    }
}

svg, path{
    color: inherit;
}

#app > main{
    display: flex;
    flex-direction: row;

    #list{
        width: 30%;
        border-right: 1px solid rgb(65,65,75);
    }

    #display, #noDisplay{
        width: 70%;
        border-left: 1px solid rgb(65,65,75);
    }
}

// @media screen and (max-width: 968px) {
//     #app > main{

//         #list, #display, #noDisplay{
//             width: 100%;
//             border: 0;
//         }

//         #display, #noDisplay{
//             display: none;
//         }
//     }
// }

</style>
