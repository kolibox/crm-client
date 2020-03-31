<template>
    <div id="app">
        <img width="25%" src="./assets/logo.png">
        <formsBlock v-bind:param1='send_me_to_child' param2='param2' param3='param3' v-on:newRow="sendNewRowToAgGrid" />
        <HelloWorld msg='kolibomber'/>
        <aggrid ref="grid"/>

        
        
   
</div>
</template>



<script>
import HelloWorld from './components/HelloWorld'
import aggrid from './components/ag-grid'
import formsBlock from './components/FormsBlock'

    export default {
        name: 'App',
        components: { 
            HelloWorld,
            aggrid,
            formsBlock
            },

        data() { 
            return {
                //numberOfNewRowsAdded: 0,
                send_me_to_child: 'dinamic'
                }

            },
        methods: {
            sendNewRowToAgGrid()
            {
                this.$refs.grid.refreshDataFromDataBase(); //Вызываем метод дочернего компонента после вычитки из базы данных.
            }
            
        
        }
    }
</script>

<style>
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smotthing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
