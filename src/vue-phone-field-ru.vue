<script>

export default /*#__PURE__*/{
  name: 'VuePhoneFieldRu', // vue component name
   props: {
    value: {
      type: String,
      default: '',
    },
    placeholder: {
      type: String,
      default: 'Телефон',
    },
    autocomplete: {
      type: String,
      default: 'off',
    },
    autofocus: {
      type: String,
      default: null,
    },
    validate: {
      type: Boolean,
      default: false,
    },
  },
  data: () => {
    return {
      isValid: null,
      touched: false
    }
  },
  methods: {
    inputHandler(event) {
      if (event.isTrusted) {
        this.touched = true
      }
      let value = event && event.target ? event.target.value : event
      const selectionStart = event && event.target ? event.target.selectionStart : null
      if (selectionStart && value.length != selectionStart) {
        // Editing in the middle of input, not last symbol
        if (event.data && /\D/g.test(event.data)) {
          // Attempt to input non-numeric symbol
          const replace = event.data
          const regEx = new RegExp(replace, 'g')
          this.$refs.phoneInput.value = value.replace(regEx, '')
          this.$refs.phoneInput.setSelectionRange(selectionStart - 1, selectionStart - 1)
        }
        this.emit(value)
        return
      }
      if (value) {
        value = value.replace(/[^0-9]/g, '')
        if (value.match(/^\+?89$/) || value.match(/^9$/)) {
          value = '79'
        }
        if (value.match(/^\+?9\d{9}$/)) {
          value = '7' + value
        }
        if (value.match(/^\+?8\d{10}$/)) {
          value = '7' + value.substring(1)
        }
        value = '+' + value
      } else {
        value = ''
      }
      value = this.formatValue(value)
      this.$refs.phoneInput.value = value
      this.emit(value)
    },
    formatValue(value) {
      if (value.length < 3) {
        return value
      }
      const firstPart = value.substring(0, 2)
      let formattedValue = ''
      if (value.length > 2) {
        formattedValue = firstPart + ' (' + value.substring(2, 5)
      }
      if (value.length >= 6) {
        formattedValue += ') ' + value.substring(5, 8)
      }
      if (value.length >= 9) {
        formattedValue += '-' + value.substring(8, 10)
      }
      if (value.length >= 11) {
        formattedValue += '-' + value.substring(10)
      }
      return formattedValue
    },
    focusHandler(e) {
      if (!this.value.length) return
      const length = this.value.length
      //timeout for different browsers support
      setTimeout(() => {
        e.target.setSelectionRange(length, length)
      }, 0)
    },
    validateFunc(value) {
      if (!this.validate || !this.touched) return
      if (/^\+7\s\(\d{3}\)\s\d{3}-\d{2}-\d{2}$/.test(value)) {
        this.isValid = true
      } else {
        this.isValid = false
      }
    },
    emit(value) {
      this.validateFunc(value)
      this.$emit('input', value)
      this.$emit('raw-value', value.replace(/[^0-9]/g, ''))
    },
  },
  mounted() {
    if (this.value) {
      this.inputHandler(this.value)
    }
    if (this.autofocus === '') {
      this.$refs.phoneInput.focus()
    }
  }
};
</script>

<template>
 <input
    type="tel"
    :value="value"
    @input="inputHandler"
    ref="phoneInput"
    :placeholder="placeholder"
    @focus="focusHandler"
    :autocomplete="autocomplete"
    :class="{ valid: isValid ? true : false, invalid: isValid === false ? true : false }"
  />
</template>

<style scoped>
input {
  font-size: 22px;
  padding: 10px;
  border: 1px solid #ccc;
  min-width: 300px;
  max-width: 400px;
  width: 90%;
  margin-bottom: 5px;
}
input:focus {
  color: #495057;
  background-color: #fff;
  border-color: #80bdff;
  outline: 0;
  box-shadow: 0 0 0 0.2rem rgb(0 123 255 / 25%);
}
.valid {
  border-color: #28a745;
}
.valid:focus {
  border-color: #28a745;
  box-shadow: 0 0 0 0.2rem rgb(40 167 69 / 25%);
}
.invalid {
  border-color: #dc3545;
}
.invalid:focus {
  border-color: #dc3545;
  box-shadow: 0 0 0 0.2rem rgb(220 53 69 / 25%);
}
</style>
