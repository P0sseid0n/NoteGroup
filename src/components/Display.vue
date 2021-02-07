<template>
<div id="display">
    <header>
        <div>
            <h1> {{ note.title }} </h1>
        </div>
        <div>
            <button><font-awesome-icon :icon="['fas', 'pen']" /> Edit</button>
            <button><font-awesome-icon :icon="['fas', 'trash']" /> Delete</button>
        </div>
    </header>
    <nav>
        <button class="displaying" id="todos" @click="changeDisplaying('todos')">Todos</button>
        <button id="notes" @click="changeDisplaying('notes')">Notes</button>
    </nav>
    <main>

    </main>
</div>
</template>

<script>
export default {
    props: {
        note: Object
    }, 
    data(){
        return{
            displaying: 'todos'
        }
    },
    methods: {
        changeDisplaying(displaying){
            this.displaying = displaying

            document.querySelector('.displaying').classList.remove('displaying')
            document.querySelector(`#${this.displaying}`).classList.add('displaying')
        }
    }
}
</script>

<style lang="scss">
#display{
    background: var(--depth-2);
    padding: 8px 24px;

    header{
        display: flex;
        align-items: center;
        justify-content: space-between;

        h1{
            font-size: 44px;
        }

        button{
            background-color: var(--blue);
            border: 0;
            outline: 0;
            font-weight: 700;
            font-size: 14px;
            padding: 8px 8px;
            border-radius: 8px;
            cursor: pointer;
            margin-right: 16px;

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

        button{
            background: transparent;
            opacity: .82;
            border: 0;
            outline: 0;
            cursor: pointer;
            margin-right: 16px;
            font-size: 18px;
            padding: 8px 4px;
            font-weight: 600;
            position: relative;
            top: 3px;

            &.displaying{
                opacity: .97;
                font-weight: bold;
                border-bottom: 4px solid var(--blue-light);
            }
        }
    }
}
</style>