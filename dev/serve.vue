<script>
import Vue from 'vue';
import VuePhoneFieldRu from '@/vue-phone-field-ru.vue';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    VuePhoneFieldRu
  },
  data() {
    return {
      phone: '+7'
    }
  },
});
</script>

<template>
  <div id="app">
    <vue-phone-field-ru v-model="phone" autofocus validate />
  </div>
</template>
