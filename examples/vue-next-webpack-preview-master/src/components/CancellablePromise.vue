<template>
  <div id="cancellablePromise">
    <div>
      <label for="delayPromise">
        Delay seconds
      </label>
      <input name="delayPromise" v-model="delay">
    </div>
    <div>
      <button @click="exec(delay)" 
              :disabled="loading">Execute</button>
      <button @click="cancel()" :disabled="!loading">
        Cancel
      </button>
    </div>

    <div v-if="loading">
      loading...
    </div>
    <div v-else-if="cancelled">
      <p>Request cancelled</p>
      <p>
        Result: {{result}}
      </p> 
      <p>
        Error: {{error}}
      </p>
    </div>
    <div v-else>
      <p>
        Result: {{result}}
      </p> 
      <p>
        Error: {{error}}
      </p>
    </div>
  </div>
</template>

<script>
import { useCancellablePromise } from 'vue-composable';
import { ref } from 'vue';

export default {

  setup(){
    const {exec, loading, cancel, error, cancelled, result} = useCancellablePromise((delay)=> fetch(`https://reqres.in/api/users?delay=${delay}`).then(x=>x.json()))
  
    const delay = ref(1);
    return {
      delay,
      exec,
      error,
      loading,
      cancel,
      cancelled,
      result
    }
  }

}
</script>
