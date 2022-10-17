<template>
<div id="wrapper">
<sidebar-component></sidebar-component>
<div id="content-wrapper" class="d-flex flex-column">
<div id="content">
<navbar-component></navbar-component>
<div class="container-fluid">
<div class="row">
<div class="col-lg-8">
<div class="card shadow mb-4">
<div class="card-header py-3">
<h6 class="m-0 font-weight-bold text-primary">Tambah Detail Transaksi</h6>
</div>
<div class="card-body">
<form @submit.prevent="tambah">
<div class="form-group">
<label>Jenis Paket</label>
<select class="form-control" v-model="detail.id_paket">
<option v-for="(p, index) in paket" :key="index" :value="p.id_paket">{{ p.jenis }}</option>
</select>
</div>
<div class="form-group">
<label>Jumlah (kg / satuan></label>
<input type="text" class="form-control" v-model="detail.qty">
</div>
<input type="hidden" v-model="detail.id_transaksi">
<button type="submit" class="btn btn-success btn-block">Simpan</button>
</form>
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
            id_transaksi : this.$route.params.id,
            paket : {},
            detail : {}
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
    this.axios.get(`http://127.0.0.1:8000/api/paket`, { headers : { 'Authorization' : `Bearer ` + this.$store.state.token} })
    .then( (res) => {
        this.paket = res.data
        this.detail.id_transaksi = this.id_transaksi
    })
    .catch(err => console.log(err))
    },
    methods : {
    tambah() {
        this.axios.post('http://127.0.0.1:8000/api/detail_transaksi',
        this.detail,
        { headers : { 'Authorization' : `Bearer ` + this.$store.state.token} })
        .then( () => {
            this.$router.push({ name : 'detailtransaksi', params : this.id_transaksi});   
        })
        .catch( err => console.log(err))
    }
    }
}
</script>