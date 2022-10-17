<template>
  <div id="wrapper">
    <sidebar-component></sidebar-component>
    <div id="content-wrapper" class="d-flex flex-column">
      <div id="content">
        <navbar-component></navbar-component>
        <div class="container-fluid">
          <h1 class="h3 mb-4 text-gray-800">Data paket</h1>
          <div class="row">
            <div class="col-lg-12">
              <div class="card shadow mb-4">
                <div class="card-body">
                  <div class="table-responsive">
                    
                    <table
                      class="table table-bordered mt-3"
                      width="100%"
                      cellspacing="0"
                    >
                      <thead>
                        <tr>
                          <th>#</th>
                          <th>Jenis</th>
                          <th>Harga</th>
                          <!-- <th>Aksi</th> -->
                        </tr>
                      </thead>
                      <tbody>
                        <tr v-for="(m, index) in paket" :key="index">
                          <td>{{ index + 1 }}</td>
                          <td>{{ m.jenis }}</td>
                          <td>{{ m.harga }}</td>
                          <!-- <td>
                             <router-link :to="{ name : 'editpaket' , params : {id_paket : m.id_paket}}" class="btn btn-warning btn-circle">
                                                            <i class="fas fa-pen"></i>
                            </router-link>
                            <button type="button" @click="hapus(m.id_paket)" class="btn btn-danger btn-circle">
                                                            <i class="fas fa-trash"></i>
                            </button>
                          </td> -->
                        </tr>
                      </tbody>
                    </table>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <footer-component></footer-component>
    </div>
  </div>
</template>

<script>
export default {
    data() {
        return {
            paket : {}
        }
    },
    created() {
      var data = JSON.parse(this.$store.state.datauser)
        var role = data.role
        if(role == 'owner')
        {
            this.$swal("Error","Anda tidak dapat mengakses halaman ini","error")
            this.$router.push('/') 
        }
        this.axios.get('http://127.0.0.1:8000/api/paket',
                        {
                            headers : { Authorization : "Bearer " + this.$store.state.token}
                        })
                    .then(res => {
                        this.paket = res.data
                    })
    },
    methods : {
       hapus(id_paket) {
      this.axios
        .delete(`http://127.0.0.1:8000/api/paket/${id_paket}`, {
          headers: { Authorization: "Bearer " + this.$store.state.token },
        })
        .then(() => {
          let i = this.paket.map((item) => item.id_paket).indexOf(id_paket);
          this.paket.splice(i, 1);
        });
        }

    }
}
</script>