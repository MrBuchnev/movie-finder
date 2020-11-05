<template>
  <form class="form" :class="{ 'has-value': inputValue.length > 0 }">
    <label class="form__label" :for="getFieldId">{{ label }}</label>

    <input
      :id="`${name}-${getFieldId}`"
      v-model="inputValue"
      class="form__input"
      :class="`form__input--${inputType}`"
      :type="inputType"
      :name="name"
      :value="value"
    />

    <button class="form__btn" type="submit" @click.stop.prevent="handleSubmit">
      <Icon class="form__btn-icon" icon="search" size="md" />
    </button>
  </form>
</template>

<script>
export default {
  props: {
    inputType: {
      type: String,
      required: true,
      default: 'text',
    },

    name: {
      type: String,
      required: true,
      default: '',
    },

    value: {
      type: String,
      required: false,
      default: '',
    },

    label: {
      type: String,
      required: false,
      default: '',
    },
  },

  data() {
    return {
      inputValue: this.value,
    }
  },

  computed: {
    getFieldId() {
      return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, (c) => {
        const r = (Math.random() * 16) | 0
        // prettier-ignore
        const v = c === 'x' ? r : (r & 0x3 | 0x8)

        return v.toString(16)
      })
    },
  },

  methods: {
    handleSubmit() {
      console.log('submitted', this.inputValue)
    },
  },
}
</script>

<style lang="scss" scoped>
.form {
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;

  &__label {
    position: absolute;
    left: 12px;
    color: $black-05;
    z-index: -1;
    transition: color 0.1s linear, transform 0.1s linear;
  }

  .has-value &__label {
    color: $black;
    transform: translateY(-36px);
  }

  &__btn {
    position: absolute;
    right: 12px;
    transition: opacity 0.1s linear;
  }

  &__input {
    width: 100%;
    background-color: $transparent;

    &--text {
      font-size: 16px;
      line-height: 42px;
      padding: 0 12px;
      border: 1px solid $black;
      border-radius: 10px;
    }
  }

  @media (hover: hover) {
    &__btn:hover {
      opacity: 0.5;
    }
  }
}
</style>
