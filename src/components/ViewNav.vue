<template>
<div id="view">
    <button id="change-list" class="viewing" @click="changeView('list')"> <h1><font-awesome-icon :icon="['fas', 'list']" /></h1> <h2>List</h2> </button>
    <div class="line"></div>
    <button id="change-display" @click="changeView('display')" > <h1><font-awesome-icon :icon="['fas', 'desktop']" /></h1> <h2>Group</h2> </button>
</div>
</template>

<script>
export default {
    methods:{
        changeView(view){
            let oldView

            if(view == 'list'){
                oldView = 'display'
            } else if(view == 'display'){
                oldView = 'list'
            } else {
                return
            }

            document.querySelector(`#change-${oldView}`).classList.remove('viewing')
            document.querySelector(`#change-${view}`).classList.add('viewing')

            document.querySelector(`#${oldView}`).style.display = 'none'
            document.querySelector(`#${view}`).style.display = 'block'

        }
    }
}
</script>

<style lang="scss">
#view{
    width: 100%;
    height: 72px;
    background: var(--depth-3);
    display: flex;
    align-items: center;

    button{
        background: transparent;
        outline: 0;
        border: 0;
        cursor: pointer;
        width: calc(50% - 2px);
        height: 100%;

        h1{
            font-size: 32px;
        }

        h2{
            font-size: 16px;
        }
        &.viewing{


            *{
                color: var(--blue);
            }
        }
    }

    .line{
        height: 80%;
        background: rgb(80,80,100);
        width: 4px;
        display: block;
        border-radius: 16px;
    }
}
</style>