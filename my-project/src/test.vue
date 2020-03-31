<template>
    <div>
   
  <ag-grid-vue style="width: 100%; height: 200px;"
                 class="ag-theme-balham"
                 :columnDefs="columnDefs"
                 :rowData="rowData">
  </ag-grid-vue>
    <p>soso</p>
   
</div>
</template>



<script>
import {AgGridVue} from "ag-grid-vue";

    export default {
        name: 'test',
        data() {
            return {
                columnDefs: null,
                rowData: null
            }
        },
        
        components: {
            AgGridVue
            
        },
        beforeMount() {
            this.columnDefs = [
                {headerName: 'Mongo ID', field: '_id', sortable: true, filter: true, checkboxSelection: true, resizable: true, width: 250},
                {headerName: 'name', field: 'name', sortable: true, filter: true, checkboxSelection: true, resizable: true, width: 250},
                {headerName: 'operator', field: 'operator', sortable: true, filter: true, checkboxSelection: true, resizable: true},
                {headerName: 'date', field: 'date', sortable: true, filter: true, checkboxSelection: true, cellRenderer: data => {
                    let value = data.value.substring(0, 10) + " " + data.value.substring(11, 19);
                    return value}, resizable: true}
                //{headerName: '__v', field: '__v', sortable: true, filter: true, checkboxSelection: true}
            ];

            //async function func(cont)
            //{
            //    let response = await fetch('http://127.0.0.1:3000/api/operators');

                //if (response.ok)
                //{
                    //let json = await response.json();
                    //cont = [{_id: 'Toyota', name: 'Celica', operator: 35000}];
                    //console.log(json[0]);
                    //return json[0];
                //}
            //}

            //console.log('func: ', func());
            //func(this.rawData);

            
            //Вот тут нужно с CORS поработать. Пока она отключена
            fetch('http://127.0.0.1:3000/api/operators')
            .then(result => result.json())
            .then(data => this.rowData = data);
        }
    }
</script>

<style lang="scss">
  @import "../node_modules/ag-grid-community/dist/styles/ag-grid.css";
  @import "../node_modules/ag-grid-community/dist/styles/ag-theme-balham.css";
</style>
