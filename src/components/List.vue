<template>
<div id="list">
    <header>
        <button @click="addGroup()" ><span><font-awesome-icon :icon="['fas', 'plus']" /></span>  Add new group</button>
    </header>
    <hr>
    <main>
        <div class="group" v-for="(note, index) in notes" :key="note.id" @click="setSelected(index)">
            <div>
                <input type="checkbox" :id="'group-' + index" :checked='note.done'>
                <label :for="'group-' + index"><font-awesome-icon :icon="['fas', 'check']" /></label>
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
    props: {
        notes: Array
    },
    methods: {
        setSelected(id){
            this.$root.$emit('setSelected')
            if(id < 0) id = 0

            this.$root.$emit('selecting')

            const target = document.querySelectorAll('.group')[id]
            const selected = document.querySelector('.selected')

            if(target.classList.contains('selected')) return

            
            target.classList.add('selected')
            if(selected) selected.classList.remove('selected')

            this.$parent.noteSelected = id

        },

        async addGroup(){
            const notes = this.$props.notes
            let id = 0

            if(notes.length > 0) id = notes[notes.length - 1].id + 1

            await this.$props.notes.push({ title: 'New Group - ' + id, id,  done: false, todos: [], notes: [] })
            this.setSelected(id)

            this.$root.$emit('saveNotes')
        }
    },
    mounted(){
        this.$root.$on('deleteGroup', group => {
            if(this.$parent.notes.length > 0) this.setSelected(group)
        })
        this.$root.$on('createGroup', this.addGroup)
    },
    created(){
        setTimeout(() => {
            if(this.$parent.notes.length > 0) this.setSelected(0)
        }, 1)
    }
}
</script>

<style lang="scss">
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
            box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;

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
        overflow-y: auto;
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
            box-shadow: rgba(0, 0, 0, 0.16) 0px 10px 36px 0px, rgba(0, 0, 0, 0.06) 0px 0px 0px 1px;

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
                    pointer-events: none;

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
                        pointer-events: none;
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
                    font-size: 16px;
                    opacity: 0.8;
                }
            }

            
        }
    }
}

</style>