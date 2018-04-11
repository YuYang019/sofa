<template>
  <div 
    :class="[
      type === 'textarea' ? 'sofa-textarea' : 'sofa-input',
      inputSize ? `sofa-input--${inputSize}` : '',
      {
        'is-disabled': inputDisabled,
      } 
    ]"
    @mouseenter="hovering === true"
    @mouseleave="hovering === false"
  >
    <input 
      :type="type"
      v-if="type != 'textarea'"
      :placeholder="placeholder"
      :disabled="inputDisabled"
      :value="inputValue"
      :autocomplete="autoComplete"
      ref="input"
      @input="handleInput"
      @focus="handleFocus"
      @blur="handleBlur"
      @change="handleChange"
    >
  </div>
</template>
<script>
  export default {
    name: 'SofaInput',

    componentName: 'SofaInput',

    data() {
      return {
        inputValue: this.value || '',
        hovering: false,
        focused: false,
      }
    },

    props: {
      value: [String, Number],
      size: String,
      disabled: Boolean,
      placeholder: {
        type: String,
        default: '',
      },
      type: {
        type: String,
        default: 'text',
      },
      autoComplete: {
        type: String,
        default: 'off',
      },
    },

    computed: {
      inputSize() {
        return this.size || ''
      },
      inputDisabled() {
        return this.disabled || false
      },
    },

    watch: {
      value(val, oldVal) {
        this.setInputValue(val)
      }
    },

    methods: {
      focus() {
        this.$refs.input.focus();
      },
      setInputValue(val) {
        this.inputValue = val;
      },
      handleInput(event) {
        const value = event.target.value;
        this.$emit('input', value);
      },
      handleFocus(event) {
        this.focused = true;
        this.$emit('focus', event);
      },
      handleBlur(event) {
        this.focused = false;
        this.$emit('blur', event);
      },
      handleChange(event) {
        const value = event.target.value;
        this.$emit('change', value);
      },
      clear() {
        this.$emit('input', '');
        this.$emit('change', '');
        this.$emit('clear');
        this.setInputValue('')
        this.focus();
      },
    },
  };
</script>