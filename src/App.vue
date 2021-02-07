<template>
    <div id="app">
        <header>
            <h1>NoteGroup</h1>
            <h4>Made by <a href="https://posseidon.netlify.app/" target="_blank" rel="noopener noreferrer">P0sseid0n</a></h4>
        </header>
        <main>
            <List :notes="notes" />
            <Display :note="notes[noteSelected]" />
        </main>
    </div>
</template>

<script>
import List from './components/List.vue'
import Display from './components/Display.vue'

export default {
    name: 'App',
    data(){
        return {
            notes: [ { title: 'Note - 1' }, { title: 'Note - 2' }, { title: 'Note - 3' } ],
            noteSelected: 0
        }
    },
    components: {
        List, 
        Display
    }
}
</script>

<style lang="scss">
:root{
    --groups: rgb(40,40,50);
    --groups-hovered: rgb(45,45,55);

    --blue-light: hsl(200, 100%, 50%);
    --blue: hsl(205, 100%, 50%);
    --blue-dark: hsl(210, 100%, 50%);

    --depth-1: rgb(30,30,40);
    --depth-2: rgb(40,40,50);
    --depth-3: rgb(50,50,60);
    --depth-4: rgb(60,60,70);
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
        width: 25%;
        border-right: 1px solid rgb(65,65,75);
    }

    #display{
        width: 75%;
        border-left: 1px solid rgb(65,65,75);
    }
}

</style>
