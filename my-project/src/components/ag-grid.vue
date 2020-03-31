<template>
    <div>
        
   <button v-on:click="setRowData()">set rowdata</button>
   <button v-on:click="clearData()">clear rowdata</button>
   <button style="visibility: hidden" v-on:click="deleteSelected()">delete selected</button>
   <button v-on:click="setComplete()">Complete</button>
   <button v-on:click="refreshAllDataFromDataBase">Show all data</button>
   <button v-on:click="refreshDataFromDataBase">Show uncomlete data</button>
  <ag-grid-vue style="width: 100%; height: 200px;"
                 class="ag-theme-balham"
                 :columnDefs="columnDefs"
                 :rowData="rowData"
                 :gridOptions="gridOptions"
                 :animateRows="true"
                 :rowClassRules="rowClassRules"
                 >
  </ag-grid-vue>
    <p>soso</p>
   
</div>
</template>



<script>
import {AgGridVue} from "ag-grid-vue";
//import {AllCommunityModules} from "@ag-grid-community/all-modules";

    export default {
        name: 'aggrid',
        data() {
            return {
                columnDefs: null,
                rowData: null,
                gridOptions: null,
                gridApi: null,
                columnApi: null,
                rowSelection: null, 
                myTestPar: 10,
                rowClassRules: null //Это для подсветки строк
                //modules: AllCommunityModules
            }
        },
        
        components: {
            AgGridVue
            },

        methods: {
            setRowData() //заглушка для тестов
            {
                this.gridApi.setRowData([{"_id":"5de9f027b0d93c209420132a","name":"Kill","operator":"Base","date":"2019-12-06T06:07:35.009Z","__v":0},{"_id":"5de9f027b0d93c209420132b","name":"Kill","operator":"Base","date":"2019-12-06T06:07:35.016Z","__v":0},{"_id":"5de9f027b0d93c209420132c","name":"Kill","operator":"Base","date":"2019-12-06T06:07:35.026Z","__v":0},{"_id":"5de9f027b0d93c209420132d","name":"Kill","operator":"Base","date":"2019-12-06T06:07:35.033Z","__v":0}]);
            },
            clearData() //заглушка для тестов
            {
                this.gridApi.setRowData([]);
            },
            deleteSelected() //Уже не актуально. Удаляем по кнопке внутри каждой строки
            {
                let selectedRaw = this.gridApi.getSelectedRows();
                console.log('deleting: ', selectedRaw[0]['_id']);
                fetch("http://localhost:3000/api/operators/" + selectedRaw[0]['_id'], {
                    method: "DELETE",
                    headers: {'Content-type': 'application/json'}, //Тут в примере еще были всякие хидеры. Даже про CORS
                        })
                .then(response => response.text())
                .then(data => console.log(data))
                .catch(error => console.log(error));

                this.gridApi.updateRowData({remove: selectedRaw});
            },
            async refreshDataFromDataBase(){ //Пропатчил. Теперь все фильтруется по полю name (врменное поле)
                fetch('http://127.0.0.1:3000/api/operators')
                .then(result => result.json())
                .then(data => {let filteredData = data.filter(x => x.name != 'kill'); let i = 1; filteredData.forEach(x => x.number = i++); this.rowData = filteredData});
            },
            async refreshAllDataFromDataBase(){ //Пропатчил. Теперь все фильтруется по полю name (врменное поле)
                fetch('http://127.0.0.1:3000/api/operators')
                .then(result => result.json())
                .then(data => {let i = 1; data.forEach(x => x.number = i++); this.rowData = data});
            },



            async testFunc()
            {
                this.myTestPar = 50;
                console.log('testFunc?');
            },
            setComplete() //Поиграться с классом всех строк
            {
                
                //this.gridOptions.rowStyle = {background: 'coral'};
                //this.gridOptions.rowClass = 'sick-days-warning';
                this.gridOptions.rowClassRules = {'sick-days-warning' : function(params) {console.log(params); return true}};
                this.gridApi.redrawRows();
            }

            
            

        },
        beforeMount() {
            this.gridOptions = {};
            this.gridApi = this.gridOptions.api;
            this.gridColumnApi = this.gridOptions.columnApi;
            //console.log(this.rowClassRules);

            
            //this.rowClassRules = {"sick-days-warning": true};
            //this.testFunc(); //Методы вызываются через this
            //console.log('test async');
            this.columnDefs = [
                {headerName: 'Номер', field: 'number', sortable: true, filter: true, resizable: true, width: 50, sortingOrder: ['asc','desc']},
                {headerName: 'Флаг завершения', field: 'p1', sortable: true, filter: true, resizable: true, width: 250, hide: true, checkboxSelection: true, },
                {headerName: 'Оператор', field: 'p2', sortable: true, filter: true, resizable: true, editable: true},
                {headerName: 'Поставщик', field: 'p3', sortable: true, filter: true, resizable: true},
                {headerName: 'Попытки', field: 'p4', sortable: true, filter: true,  resizable: true, width: 100},
                {headerName: 'Комментарии', field: 'p5', sortable: true, filter: true, resizable: true},
                {headerName: 'Тестовый клиент', field: 'p6', sortable: true, filter: true, resizable: true, width: 100},
                {headerName: 'Контакты', field: 'p7', sortable: true, filter: true, resizable: true, width: 100},
                {headerName: 'Рег. номер', field: 'p8', sortable: true, filter: true, resizable: true, width: 100},
                {headerName: 'Критерии', field: 'p9', sortable: true, filter: true, resizable: true},
                {headerName: 'p10', field: 'p10', sortable: true, filter: true, resizable: true, hide: true},
                {headerName: 'Создано', field: 'creationDate', sortable: true, filter: true, unSortIcon: true, width: 150,  cellRenderer: data => {
                    let x = new Date();
                    let currentTimeZoneOffsetInHours = -x.getTimezoneOffset() / 60;
                    let a = Number(data.value.substring(11, 13)) + currentTimeZoneOffsetInHours; // 
                    let value = data.value.substring(0, 10) + " " + a + data.value.substring(13, 19);
                    return value}, resizable: true},
                {headerName: 'Завершено', field: 'completeDate', sortable: true, filter: true, width: 150, resizable: true},
                {headerName: 'Удалить', field: '_id', cellRenderer: deleteButton, width: 85},
                {headerName: 'Завершить', field: '_id', cellRenderer: completeButton, width: 100},
                {headerName: 'Сохранить', field: '_id', cellRenderer: saveButton, width: 100}
                //{headerName: '__v', field: '__v', sortable: true, filter: true, checkboxSelection: true}
                
            ];
            
            let that = this; //Новая штука с ES6. Сохраняет контекст, т.к. this поменяется внутри функции
            
            console.log('out: ', this.myTestPar);
            function deleteButton(params)
            {
                let sButton = document.createElement('Button');
                sButton.innerHTML = 'Удалить';
                sButton.addEventListener('click', function()
                {
                    console.log('deleting: ', params.value);
                    console.log('deleting rowIndex: ', params.rowIndex);
                    fetch("http://localhost:3000/api/operators/"+params.value, {
                            method: "DELETE",
                            headers: {'Content-type': 'application/json'}, //Тут в примере еще были всякие хидеры. Даже про CORS
                        })
                        .then(response => response.text())
                        .then(data => console.log(data))
                        .catch(error => console.log(error));
                        console.log('in here');
                        console.log('in: ', that.myTestPar);
                        
                        that.gridApi.updateRowData({remove: [params.data]});
                        //Снова лезем в базу, но тут перебрасывается вверх списка фокус ввода. Некрасиво
                        //fetch('http://127.0.0.1:3000/api/operators')
                        //.then(result => result.json())
                        //.then(data => that.rowData = data);
                        //that.gridApi.setRowData([{"_id":"5de9f027b0d93c209420132a","name":"Kill","operator":"Base","date":"2019-12-06T06:07:35.009Z","__v":0},{"_id":"5de9f027b0d93c209420132b","name":"Kill","operator":"Base","date":"2019-12-06T06:07:35.016Z","__v":0},{"_id":"5de9f027b0d93c209420132c","name":"Kill","operator":"Base","date":"2019-12-06T06:07:35.026Z","__v":0},{"_id":"5de9f027b0d93c209420132d","name":"Kill","operator":"Base","date":"2019-12-06T06:07:35.033Z","__v":0}]);
                    

                        
                })
                return sButton;
            }

            function saveButton(params)
            {
                console.log('nothing');
                let sButton = document.createElement('Button');
                sButton.innerHTML = 'Сохранить';
                return sButton;

            }
            
            function completeButton(params) //Сейчас настроено на поле name. Надо будет сделать отдельное поле в базе
            //А также нужно сделать изначальное закрашивание по нужному полю в цвет "завершено" в before mount?
            {
                let sButton = document.createElement('Button');

                sButton.id = params.data._id;
                if (params.data.p1 != 'complete')
                sButton.innerHTML = 'Завершить';
                else
                sButton.innerHTML = 'Отменить';
                //console.log(params);
                sButton.addEventListener('click', () =>
                    {
                    //Логика включения/выключения.
                    
                    if (params.data.p1 != 'complete') {params.data.p1 = 'complete';}
                    else {params.data.p1 = 'notComplete';} //Вот тут доделать!!!!!
                    //if (sButton.innerHTML == 'Завершить') sButton.innerHTML = 'Отменить';
                    //else sButton.innerHTML = 'Завершить';
                    //that.gridApi.refreshCells();
                    that.gridApi.redrawRows();
                    })
                //console.log(sButton.innerHTML);
                //if (params.data.name != 'Kill') {sButton.innerHTML = 'Завершить'}
                //else {params.data.name = 'noKillPlease'; sButton.innerHTML = 'Отменить'} //Вот тут доделать!!!!!

                //console.log('sbutton return: ', sButton);
                
                return sButton;
            }
            

            
            this.rowClassRules = {'sick-days-warning' : function(a) {if (a.data.p1 == 'complete') {return true}}};//Тут реально все проверяем, а не только текущий
            

            this.refreshDataFromDataBase(); //Вот тут первый запрос к базе при загрузке страницы
            

            //ВОТ ЭТО РЕАЛЬНО РАБОТАЕТ. вариант из мануала 
            //Вот тут нужно с CORS поработать. Пока она отключена
            //fetch('http://127.0.0.1:3000/api/operators')
            //.then(result => result.json())
            //.then(data => this.rowData = data);
        },
        mounted() {
            this.gridApi = this.gridOptions.api;
            this.gridColumnApi = this.gridOptions.columnApi;

            //Подкрашиваем завершенные строки (пока по полю name)
            //this.gridOptions.rowClassRules = {'sick-days-warning' : function(a) {if (a.data.name == 'Kill') {return true}}};
            //this.gridApi.redrawRows();
            
        }
        
        

    }

</script>

<style lang="scss">
  @import "../../node_modules/ag-grid-community/dist/styles/ag-grid.css";
  @import "../../node_modules/ag-grid-community/dist/styles/ag-theme-balham.css";
  .ag-row-hover{
      background-color: #dfdfff !important;
  }
  .ag-column-hover {
      background-color: #dfffdf;
  }
  .ag-theme-balham .sick-days-warning {
      background-color: rgb(169, 245, 150) !important;
  }
</style>
