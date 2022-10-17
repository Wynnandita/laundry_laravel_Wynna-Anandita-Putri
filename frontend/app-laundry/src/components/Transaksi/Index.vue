<template>
    <div id="wrapper">
        <sidebar-component></sidebar-component>
        <div id="content-wrapper" class="d-flex flex-column">
            <div id="content">
                <navbar-component></navbar-component>
                
                <div class="container-fluid">
                    <!-- Page Heading -->
                    <h1 class="h3 mb-4 text-gray-800">Data Transaksi</h1>

                    <div class="row">
                        <div class="col-lg-12">
                            <div class="card shadow mb-4">                                
                                <div class="card-body">
                                    <div class="table-responsive">
                                        <router-link to="/transaksi/tambah" class="btn btn-info btn-icon-split">
                                            <span class="icon text-white-50">
                                                <i class="fas fa-plus"></i>
                                            </span>
                                            <span class="text">Tambah</span>                                        
                                        </router-link>                                        
                                        <table class="table table-bordered mt-3" width="100%" cellspacing="0">
                                            <thead>
                                                <tr>
                                                    <th>#</th>
                                                    <th>Nama Member</th>
                                                    <th>Tgl Order</th>
                                                    <th>Status</th> 
                                                    <th>Dibayar</th>
                                                    <th>Petugas</th>
                                                    <th>Aksi</th>
                                                </tr>
                                            </thead>
                                            <tbody>
                                               <tr v-for="(t, index) in transaksi" :key="index">
                                                   <td>{{ index + 1 }}</td>
                                                   <td>{{ t.nama_member }}</td>
                                                   <td>{{ t.tanggal }}</td>
                                                   <td>
                                                        <span v-if="t.status == 'baru'" class="badge bg-info text-light">Baru</span>
                                                      <span v-if="t.status == 'proses'" class="badge bg-warning text-dark">Proses</span>
                                                      <span v-if="t.status == 'selesai'" class="badge bg-success text-light">Selesai</span>
                                                      <span v-if="t.status == 'diambil'" class="badge bg-secondary text-light">Diambil</span>
                                                   </td>
                                                   <td>
                                                        <span v-if="t.dibayar == 'dibayar'" class="badge bg-info text-light">Di Bayar</span>
                                                        <span v-if="t.dibayar == 'belum_dibayar'" class="badge bg-warning text-dark">Belum Dibayar</span>
                                                        </td>
                                                   <td>{{ t.name }}</td>
                                                   <td>
                                                       <router-link class="btn btn-success btn-circle mr-1" :to="{ name : 'detailtransaksi' , params : { id : t.id_transaksi } }">
                                                           <i class="far fa-eye"></i>
                                                        </router-link>
                                                   </td>
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
        transaksi : {}
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

    this.axios.get('http://127.0.0.1:8000/api/transaksi', {headers : { Authorization : 'Bearer ' + this.$store.state.token}})
    .then(res => {
        console.log(res.data.data)
        this.transaksi = res.data.data
    })


    },
}
</script>