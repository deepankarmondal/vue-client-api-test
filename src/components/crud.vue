<template>
  <div class="">
    <div class="text-right" style="margin-bottom:20px">
      <button type="button" class="btn btn-primary btn-sm pull-right" data-toggle="modal" data-target="#myModal">
        Add Data
      </button>
    </div>
    <div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog modal-lg" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Create</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <form class="form-inline" @submit.prevent="createData">
          <div class="modal-body">
              <label class="sr-only" for="inlineFormInput">Name</label>
              <input type="text" class="form-control mb-2 mr-sm-2 mb-sm-0" id="inlineFormInput" placeholder="Name" v-model="create.name">

              <label class="sr-only" for="inlineFormInputGroup">Contact  Number</label>
              <input type="text" class="form-control" id="inlineFormInputGroup" placeholder="Contact number" v-model="create.contact">
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
            <button type="submit" class="btn btn-primary">Save</button>
          </div>
          </form>
        </div>
      </div>
    </div>
    <table class="table table-striped">
          <thead  class="thead-inverse">
            <tr>
              <th>#</th>
              <th >Name</th>
              <th >Contact</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody>
            <template v-if="listData.length > 0">
              <tr v-for="(value,index) in listData">
                <th> {{ 1 + index}}</th>
                <td align="left">
                  <button type="button" class="btn btn-link btn-sm pull-right" data-toggle="modal" data-target="#edit" @click="editDataSet(index)"> {{ value.name}} </button>
                </td>
                <td align="left">
                    {{ value.contact}}
                </td>
                <td align="left">
                    <button type="button" name="button" class="btn btn-xs btn-danger" @click="deleteTest(index)">Delete</button>
                </td>
              </tr>
            </template>


            <tr v-else-if="listData.length == 0">
                <td colspan="4" align="center"> No data</td>
            </tr>

          </tbody>
    </table>
    <div class="modal fade" id="edit" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog modal-lg" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Edit</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <form class="form-inline" @submit.prevent="updateData()">
          <div class="modal-body">

            <label class="sr-only" for="inlineFormInput">Name</label>
            <input type="text" class="form-control mb-2 mr-sm-2 mb-sm-0" id="inlineFormInput" placeholder="Name" v-model="editData.name">

            <label class="sr-only" for="inlineFormInputGroup">Contact  Number</label>
            <input type="text" class="form-control" id="inlineFormInputGroup" placeholder="Contact number" v-model="editData.contact">

          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
            <button type="submit" class="btn btn-primary">Update</button>
          </div>
          </form>
        </div>
      </div>
    </div>
  </div>

</template>
<script>
 import axios from 'axios'
  export default{
    data() {
      return {
         create: { contact : '', name:'' },
         editData:'',
         listData:[]
      }
    },
    mounted(){
      this.fetchData();
    },
    methods: {
      fetchData(){
        let vm = this;
        axios.get('http://api.deepankarmondal.com/api/test')
        .then(function(response){
            vm.listData =  response.data;
            console.log(response)
        })
      },
      createData(){
        let vm = this;
        console.log(this.create);
        axios.post('http://api.deepankarmondal.com/api/test',vm.create)
          .then(function(response){
              //console.log(response)
              if(response.data.success)
                  vm.fetchData();
          })
      },
      editDataSet(index){
        this.editData  = this.listData[index];
      },
      updateData(){
        //console.log(this.editData);
        axios.put('http://api.deepankarmondal.com/api/test/'+this.editData.id,this.editData)
            .then(response => {
              console.log(response);
            })
      },
      deleteTest(index){
        let vm = this;
        var data = this.listData[index];
        console.log(data);
        axios.delete('http://api.deepankarmondal.com/api/test/'+data.id)
        .then(response =>{

          if(response.data.success)
              vm.listData.splice(index,1);
        })
      }
    }
  }
</script>
