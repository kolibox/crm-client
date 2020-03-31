<template>
  <div class="hello">
    <p>Put forms in here</p>
    <form @submit="formSubmit">
            <label>{{param1}} 1</label><br>
            <input type="text" name="name" class="form-control" v-model="p1"/><br><br>
            <label>{{param2}} 2</label><br>
            <input type="text" name="operator" class="form-control" v-model="p2"/><br><br>
            <label>{{param3}} 3</label><br>
            <input type="text" name="comments" class="form-control" v-model="p3"/><br><br>
            <label>{{param3}} 4</label><br>
            <input type="text" name="comments" class="form-control" v-model="p4"/><br><br>
            <label>{{param3}} 5</label><br>
            <input type="text" name="comments" class="form-control" v-model="p5"/><br><br>
            <label>{{param3}} 6</label><br>
            <input type="text" name="comments" class="form-control" v-model="p6"/><br><br>
            <label>{{param3}} 7</label><br>
            <input type="text" name="comments" class="form-control" v-model="p7"/><br><br>
            <label>{{param3}} 8</label><br>
            <input type="text" name="comments" class="form-control" v-model="p8"/><br><br>
            <label>{{param3}} 9</label><br>
            <input type="text" name="comments" class="form-control" v-model="p9"/><br><br>
            <label>{{param3}} 10</label><br>
            <input type="text" name="comments" class="form-control" v-model="p10"/><br><br>
            <button class="btn btn-primary">Отправить</button>
    </form>
  </div>
</template>

<script>

export default {
  name: 'formsBlock',
  props: {   //Это просто прикол. Проверки передачи параметров от родителя к потомку
      param1: String,
      param2: String,
      param3: String

  },
  

    data() {
            return {
                p1: '',
                p2: '',
                p3: '',
                p4: '',
                p5: '',
                p6: '',
                p7: '',
                p8: '',
                p9: '',
                p10: ''
            }
        },
    
    methods: {
        formSubmit(e)
                    {


                        //Залили новый документ через форму в базу:
                        e.preventDefault();  //А это что?

                        fetch("http://localhost:3000/", {
                            method: "POST",
                            headers: {'Content-type': 'application/json'}, //Тут в примере еще были всякие хидеры. Даже про CORS
                            body: JSON.stringify({p1: this.p1, p2: this.p2, p3: this.p3, p4: this.p4, p5: this.p5, p6: this.p6, p7: this.p7, p8: this.p8, p9: this.p9, p10: this.p10})
                        })
                        .then(response => {
                            if (!response.ok) throw Error(response.statusText);
                            return response.text();
                        })
                        .then(data => console.log(data))
                        .catch(error => console.log(error));

                        this.$emit('newRow'); //Шлем родителю эвент о добавлении новой строки в базу
                        
                    }                    


                }
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

</style>
