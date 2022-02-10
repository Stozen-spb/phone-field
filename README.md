A vue.js (vue2) phone text field component with mask +7 (969) 555-66-77 
Поле телефона корректно обрабатываем вставку и ввод телефонов в различных форматах типа:

- 8956...
- +7956...
- 7956...
- 956...

Добавляет/убирает символы и навешивает маску. Есть автофокус и валидация.
Вдохновлено этим видео https://www.youtube.com/watch?v=Lxj_v5z0xRE

Установка:
npm i vue-phone-field-ru

Использование:

<template>
 <phone-field v-model="phone" class="yourClasses" validate autofocus />
</template>
<script>
import phoneField from 'vue-phone-field-ru'
export default {
  data: {
    phone:'+7'
  },
  components: {
    phoneField,
  },
}
</script>    




