<template>
  <div >
    <div >
      <h2 class="row justify-content-center my-4">
        Bienvenido
      </h2> 
      <p class="row justify-content-center"> 
        Por favor ingrese los datos solicitados
      </p>     
    </div>
    <div class="row"> 
      <div class="card col-12 col-md-6">
        <div class="card-body row justify-content-center">
          <h5 class="card-title col-12">Datos de las personas</h5>
          <div class="col-12 col-md-5 mx-2">
            <label for="personLightMeter">Persona con contador</label>
            <input
            v-model="personLightMeter"
            class="form-control"
            id="personLightMeter" 
            type="text">
          </div>
          <div class="col-12 col-md-5 mx-2">
            <label for="normalPerson">Persona sin contador</label>
            <input
            v-model="normalPerson"
            class="form-control"
            id="normalPerson" 
            type="text">
          </div>
        </div>
      </div>
      <div class="card col-12 col-md-6">
        <div class="card-body row justify-content-center">          
          <h5 class="card-title col-12">Datos del recibo</h5>   
          <div class="col-12 col-md-6">
            <label for="totalValue">Valor total recibo</label>
            <div class="input-group">
              <input
              v-model="totalValue"
              class="form-control"
              id="totalValue" 
              type="number">
              <button
              @click="openModal('totalValue')" 
              class="btn btn-outline-primary" 
              type="button">
              <i class="fas fa-question"></i>
              </button>
            </div>
            
          </div>                 
          <div class="col-12 col-md-6">
            <label for="totalKW">Total KW</label>
            <div class="input-group">
              <input
              v-model="totalKW"
              class="form-control"
              id="totalKW" 
              type="number">
              <button
              @click="openModal('totalKW')" 
              class="btn btn-outline-primary" 
              type="button">
              <i class="fas fa-question"></i>
              </button>
            </div>
          </div>
          <div class="col-12 col-md-6">
            <label for="lastKw">KW mes pasado</label>
            <div class="input-group">
              <input
              v-model="lastKw"
              class="form-control"
              id="lastKw" 
              type="number">
              <button
              @click="openModal('kw')" 
              class="btn btn-outline-primary" 
              type="button">
              <i class="fas fa-question"></i>
              </button>
            </div> 
          </div>
          <div class="col-12 col-md-6">
            <label for="actualKw">KW mes actual</label>
            <div class="input-group">
              <input
              v-model="actualKw"
              class="form-control"
              id="actualKw" 
              type="number">
              <button
              @click="openModal('kw')" 
              class="btn btn-outline-primary" 
              type="button">
              <i class="fas fa-question"></i>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    
    <div class="row justify-content-center">
      <button
      @click="calculate()" 
      type="button"
      class="btn btn-primary mt-2">Realizar calculos</button>
    </div>
    <div class="row justify-content-start ml-2">
      <p>{{personLightMeter}} tuvo un consumo de {{}}</p>
    </div>
    <b-modal
    v-model="visibilityHelpModal" 
    id="help-modal" 
    title="Ayuda" 
    hide-footer
    size="xl">
    <div class="row justify-content-center">
      <img :src="imgRoute" alt=""
      width="100%">      
    </div>
      
    </b-modal>
  </div>
</template>

<script>
export default {
  data(){
    return{
      visibilityHelpModal : false,
      personLightMeter    : '',
      normalPerson        : '',
      totalValue          : '',
      totalKW             : '',
      lastKw              : '',
      actualKw            : '',
      imgRoute            : '',
      kwUnitValue         : '',
      kwByPersonLM        : '',
      kwByNormalP         : '',
      valuePersonLM       : '',
      valueNormalP        : ''

    } 
  },
  methods:{
    openModal(condicion){
      if(condicion==='totalValue'){
        this.imgRoute=''
      }else if(condicion==='totalKW'){
        this.imgRoute=''
      }else {
        this.imgRoute=''
      }
      this.visibilityHelpModal=true
    },
    calculate(){
      if(this.personLightMeter==""
      ||  this.normalPerson===""
      ||  this.totalValue===""
      ||  this.totalKW===""
      ||  this.lastKw===""
      ||  this.actualKw===""){
        this.$swal.fire({
          title: 'Error!',
          text: 'Diligencie todos los campos',
          icon: 'error',
          confirmButtonText: 'OK'
        })
      }else{
        this.kwUnitValue  = this.totalValue / this.totalKW
        this.kwByPersonLM = this.actualKw-this.lastKw
        if(this.kwByPersonLM<0)
        {
          this.$swal.fire({
            title: 'Error!',
            text: 'El valor de los KW de el mes actual no pueden ser menores que el mes pasado',
            icon: 'error',
            confirmButtonText: 'OK'
          })
        }else{
          this.kwByNormalP    = this.totalKW - this.kwByPersonLM
          this.valuePersonLM  = this.kwUnitValue  * this.kwByPersonLM
          this.valueNormalP   = this.kwUnitValue  * this.kwByNormalP 
        }
        console.log(this.kwUnitValue)
      }
      
    }
  }
}

</script>

<style>

</style>
