<template>
<div id="list">
    <header>
        <button @click="addNote()" ><span><font-awesome-icon :icon="['fas', 'plus']" /></span>  Add new group</button>
    </header>
    <hr>
    <main>
        <div class="group" v-for="(note, index) in notes" :key="index">
            <div>
                <input type="checkbox" :id="'checkbox-' + index">
                <label :for="'checkbox-' + index"><font-awesome-icon :icon="['fas', 'check']" /></label>
            </div>
            <div>
                <h1> {{ note.title }} </h1>
            </div>
        </div>
    </main>
</div>
</template>

<script>
export default {
    data(){
        return{
            groups: [ [ 'Note - 1', 'Description - 1' ], [ 'Note - 2', 'Description - 2' ] ]
        }
    },
    props: {
        notes: Array
    },
    methods: {
        addListeners(){
            document.querySelectorAll('.group').forEach((group, index) => {
                group.addEventListener('click', () => {
                    this.setSelected(index)
                })
            })
        },

        setSelected(id){
            const target = document.querySelectorAll('.group')[id]
            const selected = document.querySelector('.selected')

            if(target.classList.contains('selected')) return

            
            target.classList.add('selected')
            if(selected) selected.classList.remove('selected')

            this.$parent.noteSelected = id
        },

        async addNote(){
            const newId = this.$props.notes.length + 1
            await this.$props.notes.push({ title: 'Note - ' + newId })
            this.addListeners()
            this.setSelected(newId - 1)
        }
    },
    created(){
        setTimeout(() => {
            this.addListeners()
            this.setSelected(0)
        }, 1)
    }
}
</script>

<style lang="scss">

::-webkit-scrollbar {
    width: 10px;
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
#list{
    background-color: var(--depth-1);
    height: 100%;
    padding: 0 4px;
    padding-top: 8px;

    > header{
        height: 48px;
        padding: 0 4px ;

        button{
            width: 100%;
            height: 100%;
            color: white;
            background-color: var(--blue);
            border: 0;
            outline: 0;
            cursor: pointer;
            font-weight: 700;
            text-transform: uppercase;
            border-radius: 12px;

            &:hover{
                transition: .2s;
                background-color: var(--blue-light);
            }

            &:active{
                transition: 0s;
                background-color: var(--blue-dark);
            }

            span{
                font-size: 16px;
            }
        }
    }

    hr{
        margin-top: 16px;
        border: 1px solid rgb(65,65,75);
    }

    > main{
        height: calc(100% - 66px);
        overflow: auto;
        padding: 0 4px;
        padding-top: 16px;

        .group{
            cursor: pointer;
            background-color: var(--groups);
            margin-bottom: 16px;
            padding: 0 16px;
            height: 48px;
            display: flex;
            flex-direction: row;
            align-items: center;
            border-radius: 12px;

            &:hover{
                background-color: var(--groups-hovered);
            }

            &.selected{
                background-color: var(--blue);

                div:last-child{
                    h1{
                        opacity: 1;
                    }
                }
            }

            div:first-child{
                user-select: none;

                input{
                    display: none;

                    ~ label{
                        display: block;
                        width: 32px;
                        height: 32px;
                        border: 3px solid rgb(100,100,100);
                        border-radius: 25%;
                        display: flex;
                        justify-content: center;
                        align-items: center;
                        font-size: 20px;
                        color: transparent;
                        transition: 0.5s;
                        margin-right: 16px;
                    }
                }

                input:checked{
                    ~ label{
                        color: var(--blue-dark);
                        background-color: rgb(240,240,240);
                        border: 4px solid rgb(240,240,240);
                    }
                }
            }

            div:last-child{
                h1{
                    font-size: 20px;
                    opacity: 0.8;
                }
            }

            
        }
    }
}

</style>