<template>
  <q-page
    class="flex flex-center text-white"
    v-touch-pan.vertical.prevent.mouse="handlePan"
    >
    <div class="row">
      <q-input
        v-model="data.name"
        input-class="text-center text-h5 text-white"
        color="teal"
        filled
        placeholder="Counter" />
    </div>
    <div class="row full-width items-center">
      <div class="col text-center">
        <q-btn
          @click="decreaseCounter"
          v-touch-repeat:300:300:300:300:50.mouse="decreaseCounter"
          rounded
          icon="remove"
          size="xl" />
      </div>
      <div class="col text-center text-h2">
        {{data.counter}}
      </div>
      <div class="col text-center">
        <q-btn
          @click="increaseCounter"
          v-touch-repeat:300:300:300:300:50.mouse="increaseCounter"
          rounded
          icon="add"
          size="xl" />
      </div>
    </div>
    <div class="row">
        <q-btn
          @click="resetCounter"
          rounded
          icon="restart_alt"
          size="xl" />
      </div>

  </q-page>
</template>

<style scoped>
  .q-page {
    max-width: 700px;
    margin: 0 auto;
  }
</style>

<script setup>
  /*
    imports
  */

  import { reactive, watch } from 'vue'
  import { useQuasar } from 'quasar'

  /*
    quasar
  */


  const $q = useQuasar()


  /*
    data
  */
  const data = reactive({
    counter: 0,
    name: ''
  })

  const savedData = $q.localStorage.getItem('data')

  if (savedData) {
    Object.assign(data, savedData)
  }

  watch(data, value => {
    // console.log(value)
    $q.localStorage.set('data', value)
  })

  /*
    counter methods
  */
  const increaseCounter = () => {
    data.counter++
  }

  const decreaseCounter = () => {
    if(data.counter > 0) {
      data.counter--
    }
  }

  const resetCounter = () => {
    data.counter = 0
  }

  /**
    touch pan
   *
   */
  const handlePan = e => {
    // console.log(e.delta.y)
    if (e.delta.y < 0) {
      increaseCounter()
    } else {
      decreaseCounter()
    }
  }

</script>
