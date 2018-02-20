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
            </tr>
          </thead>
          <tbody>
            <tr v-if="listData.length > 0">
              <th>1</th>
              <td align="left">
                <button type="button" class="btn btn-link btn-sm pull-right" data-toggle="modal" data-target="#edit"> Mark </button>
              </td>
              <td align="left">
                <button type="button" class="btn btn-link btn-sm pull-right" data-toggle="modal" data-target="#edit"> Mark </button>
              </td>
            </tr>
            <tr v-else-if="listData.length == 0">
                <td colspan="3" align="center"> No data</td>
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
          <form class="form-inline">
          <div class="modal-body">

            <label class="sr-only" for="inlineFormInput">Name</label>
            <input type="text" class="form-control mb-2 mr-sm-2 mb-sm-0" id="inlineFormInput" placeholder="Name">

            <label class="sr-only" for="inlineFormInputGroup">Contact  Number</label>
            <input type="text" class="form-control" id="inlineFormInputGroup" placeholder="Contact number">

          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
            <button type="button" class="btn btn-primary">Update</button>
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
      //this.fetchData();
    },
    methods: {
      fetchData(){
        let vm = this;
        axios.get('http://127.0.0.1:8000/')
        .then(function(response){
            vm.listData =  response.data;
            console.log(response)
        })
      },
      createData(){
        let vm = this;
        axios.post('http://127.0.0.1:8000/',this.create)
          .then(function(response){
              console.log(response)
          })
      },
      editDataSet(index){
        this.editData  = this.listData[index];
      },
      updateData(){
        axios.put('http://127.0.0.1:8000/'+this.editData.id,this.editData)
            .then(response => {
              console.log(response);
            })
      }
    }
  }
</script>
