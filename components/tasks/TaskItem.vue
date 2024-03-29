<template>
  <li class="task-item" :class="isDoneClass">
    <span class="task-item__handler"></span>

    <c-checkbox
      class="task-item__checkbox"
      :value="task.done"
      @change="changeHandler"
    ></c-checkbox>

    <p class="task-item__value">
      {{ task.value }}
    </p>

    <div class="task-item__buttons-box">
      <button
        class="task-item__button"
        @click="editHandler"
      >
        <c-img
          class="task-item__icon task-item__icon--edit"
          :src="require('@/assets/icons/edit.svg')"
        ></c-img>
      </button>

      <button
        class="task-item__button"
        @click="deleteHandler"
      >
        <c-img
          class="task-item__icon task-item__icon--delete"
          :src="require('@/assets/icons/delete.svg')"
        ></c-img>
      </button>
    </div>
  </li>
</template>

<script>
import { mapMutations, mapActions } from 'vuex'
import CCheckbox from '@/components/common/CCheckbox'
import CImg from '@/components/common/CImg'

export default {
  name: 'TaskItem',

  components: {
    CCheckbox,
    CImg,
  },

  props: {
    task: {
      type: Object,
      required: true,
    }
  },

  computed: {
    isDoneClass () {
      const { done } = this.task

      return [
        done && 'task-item--done'
      ]
    },
  },

  methods: {
    ...mapMutations('form', [
      'setEditableTaskId',
    ]),

    ...mapActions('tasks', [
      'removeTask',
      'updateTaskState',
    ]),

    changeHandler (value) {
      this.updateTaskState({
        id: this.task.id,
        value,
      })
    },

    editHandler () {
      this.setEditableTaskId(this.task.id)
    },

    deleteHandler () {
      this.removeTask(this.task.id)
    },
  },
}
</script>

<style lang="scss" scoped>
.task-item {
  $this: &;

  position: relative;
  display: flex;
  align-items: flex-start;

  width: 100%;
  padding: 0 16px 0 20px;

  &--done {
    #{$this}__value {
      opacity: .5;
    }
  }

  &__handler {
    position: absolute;
    top: 1px;
    left: 0;
    width: 10px;
    height: 13px;
    background: url('@/assets/icons/drag.svg');
    opacity: 0;
    cursor: grab;
    transition: opacity .1s ease-in-out;
  }

  &:hover {
    #{$this}__handler {
      opacity: .5;
    }
  }

  &:active {
    #{$this}__handler {
      cursor: grabbing;
    }
  }

  &__checkbox {
    margin-right: 10px;
  }

  &__value {
    max-width: 320px;
    margin-right: auto;

    line-height: 1;
    word-wrap: break-word;
    transition: opacity .2s ease-in-out;
  }

  &__buttons-box {
    display: flex;
  }

  &__button {
    &:not(:last-child) {
      margin-right: 16px;
    }
  }

  &__button,
  &__icon {
    width: 16px;
    height: 16px;
  }
}
</style>
