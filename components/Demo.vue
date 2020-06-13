<template>
  <div class="dropdown" @focus="activate()" @blur="deactivate()" @keyup.esc="deactivate()">
    <div class="dropdown__header" :class="{'is-active' : isActive}">
      <input
        ref="dropdownInput"
        v-model="selectedLabel"
        class="dropdown__box"
        type="text"
        @focus.prevent="activate()"
        @blur.prevent="deactivate()"
        @keyup.esc="deactivate()"
      >

      <div class="dropdown__button" @mousedown.prevent.stop="toggle()">
        <i class="fas fa-angle-down" aria-hidden="true"></i>
        <i class="fas fa-angle-up" aria-hidden="true"></i>
      </div>
    </div>

    <div
      class="dropdown__content"
      v-show="isActive"
      @focus="activate"
      tabindex="-1"
      @mousedown.prevent
    >
      <ul class="dropdown__options">
        <li class="dropdown__item" :class="{'is-selected' : selectedLabel === '12:00 AM'}">
          <span @click.stop="option_onClick('12:00 AM')">12:00 AM</span>
        </li>
        <li
          class="dropdown__item"
          :class="{'is-selected' : selectedLabel === 'Two'}"
          @click.stop="option_onClick('Two')"
        >Two</li>
        <li
          class="dropdown__item"
          :class="{'is-selected' : selectedLabel === 'Three'}"
          @click.stop="option_onClick('Three')"
        >Three</li>
        <li
          class="dropdown__item"
          :class="{'is-selected' : selectedLabel === 'Four'}"
          @click.stop="option_onClick('Four')"
        >Four</li>
        <li
          class="dropdown__item"
          :class="{'is-selected' : selectedLabel === 'Five'}"
          @click.stop="option_onClick('Five')"
        >Five</li>
      </ul>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      previousTarget: null,
      selectedLabel: this.getOptionLabel(this.value),
      isActive: false
    };
  },
  methods: {
    activate() {
      if (this.isActive) return;

      this.isActive = true;
      this.$refs.dropdownInput.focus();
    },
    deactivate() {
      if (!this.isActive) return;

      this.isActive = false;
      this.$refs.dropdownInput.blur();
    },
    toggle() {
      this.isActive ? this.deactivate() : this.activate();
    },
    dropdown_onClick() {
      //event.currentTarget.classList.toggle("is-active");
      this.isActive = true;
    },
    dropdown_onKeyup() {},
    option_onClick(option) {
      this.selectedLabel = option;
      this.$refs.dropdownInput.blur();

      //this.$emit("input", option);
    },
    getOptionLabel(option) {
      return option[this.label];
    }
  },
  props: {
    label: {
      type: String
    },
    options: {
      type: Array
    },
    trackBy: {
      type: String
    },
    value: {
      type: null,
      default() {
        return [];
      }
    }
  }
};
</script>
<style lang="scss">
.dropdown {
  &__header {
    .dropdown__box {
      border: 2px solid #e2e8f0;
      padding: 0.5rem;
      width: 100%;

      &[disabled] {
        color: #E2E8F0;
      }
    }

    .dropdown__button {
      position: absolute;
      right: 3px;
      top: 50%;
      transform: translateY(-50%);
      height: 30px;
      width: 24px;

      &:hover {
        background-color: #ebf8ff;
      }

      &.disabled {
        color: #e2e8f0;

        &:hover {
          background-color: transparent;
        }
      }

      i.fas {
        position: absolute;
        right: 7px;
        top: 50%;
        transform: translateY(-50%);
        transition: opacity 0.3s;

        &.fa-angle-up {
          opacity: 0;
        }
      }
    }

    &.is-active {
      .dropdown__button {
        i.fas {
          &.fa-angle-up {
            opacity: 1;
          }

          &.fa-angle-down {
            opacity: 0;
          }
        }
      }

      + .dropdown__content {
        height: auto;
        opacity: 1;
        visibility: visible;
        width: auto;
      }
    }
  }

  &__content {
    background: #ffffff;
    border: 2px solid #e2e8f0;
    box-shadow: 0 10px 16px 0 rgba(0, 0, 0, 0.2),
      0 6px 20px 0 rgba(0, 0, 0, 0.19) !important;
    height: 0;
    margin-top: 2px;
    opacity: 0;
    overflow: hidden;
    position: absolute;
    transition: opacity 0.3s;
    visibility: hidden;

    .dropdown__options {
      list-style: none;
      margin: 0px;
      max-height: 128px;
      overflow-y: scroll;
      padding: 0px;

      .dropdown__item {
        cursor: pointer;
        padding: 4px 16px 4px 8px;
        white-space: nowrap;

        &:hover {
          background-color: #ebf8ff;
        }
        &.is-selected {
          background-color: #63b3ed;
          color: #ffffff;
        }
      }
    }
  }
}
</style>