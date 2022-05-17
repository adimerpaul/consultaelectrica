<template>
  <q-page class="">
    <div class="row">
      <div class="col-12">
        <q-form @submit.prevent="buscar">
          <div class="row">
            <div class="col-12 col-md-10 q-pa-xs">
              <q-input  outlined dense label="Ingrese numero de poste" v-model="codigo"/>
            </div>
            <div class="col-12 col-md-2 q-pa-xs flex flex-center">
              <q-btn class="full-width" label="Buscar" color="primary" icon="send" type="submit" />
            </div>
          </div>
        </q-form>
      </div>
      <div class="col-12 col-md-12">
        <q-banner dense class="bg-primary text-white">
          Datos de del poste
        </q-banner>
      </div>
      <div class="col-12 col-md-12">
        <q-table title="Datos del poste" :rows="datos" :columns="columns">
          <template v-slot:body-cell-estado="props">
            <q-td :props="props">
              <q-badge :color="props.row.estado=='EN ESPERA'?'warning':'positive'" :label="props.row.estado"/>
            </q-td>
          </template>
        </q-table>
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  data(){
    return{
      columns:[
        {label:'nombre',field:'nombre',name:'nombre',align:'left'},
        // {label:'ci',field:'ci',name:'ci',align:'left'},
        // {label:'telefono',field:'telefono',name:'telefono',align:'left'},
        {label:'reclamo',field:'reclamo',name:'reclamo'},
        {label:'estado',field:'estado',name:'estado'},
        {label:'fecha',field:'fecha',name:'fecha'},
        // {label:'hora',field:'hora',name:'hora'},
        {label:'tecnico',field:'tecnico',name:'tecnico'},
        {label:'actividad',field:'actividad',name:'actividad'},
        {label:'fechaman',field:'fechaman',name:'fechaman'},

      ],
      url:process.env.API,
      codigo:'',
      email:{},
      mails:[],
      mails2:[],
      datos:[]
    }
  },
  created() {
    // this.$q.loading.show()
    // this.$axios.get(process.env.API+'/todos').then(res=>{
    //   this.mails=[{label:process.env.API}]
    //   res.data.forEach(r=>{
    //     // console.log(r)
    //     r.label=r.codigo+' '+r.citecontrol+' '+r.remitente+' '+r.ref
    //     this.mails.push(r)
    //     // this.mails2.push(r)
    //   })
    //   this.mails2=this.mails
    //   this.codigo=this.mails[0]
    //   this.$q.loading.hide()
    // }).catch(err=>{
    //   this.$q.loading.hide()
    //   this.$q.notify({
    //     message:err.response.data.message,
    //     color:'red',
    //     icon:'error'
    //   })
    // })
  },
  methods:{
    buscar(){
      if (this.codigo==''){
        return false
      }
      this.$q.loading.show()
      this.email={}
      // console.log(this.codigo)
      this.$axios.get(process.env.API+'consulta/'+this.codigo).then(res=>{
        console.log(res.data)
        this.datos=res.data
        // if (res.data.length>0)
          // this.email=res.data[0]
        this.$q.loading.hide()
      })
        .catch(err=>{
          this.$q.loading.hide()
          console.error(err.response.data.message)
        })
    },
    filterFn (val, update) {
      if (val === '') {
        update(() => {
          this.mails = this.mails2
          // here you have access to "ref" which
          // is the Vue reference of the QSelect
        })
        return
      }
      update(() => {
        const needle = val.toLowerCase()
        this.mails = this.mails2.filter(v => v.label.toLowerCase().indexOf(needle) > -1)
      })
    }
  }
}
</script>
