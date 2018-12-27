<template>
  <div class="tasks__card" :class="{ sprint: stage === 2 }">
    <span class="tasks__card-title">{{ task.title }}</span>
    <span class="tasks__card-description">{{ task.description }}</span>
    <div v-if="stage === 2">
        <Tag v-for="tag in task.tags" :key="tag.uid" :tag="tag" :disabled="true" />
    </div>
    <div v-if="stage === 2" class="tasks__card-people">
        <div class="tasks__card-people--add" v-for="dev in task.estimations" :key="dev"></div>
        <div class="tasks__card-people--add" @click="addEstimation"></div>
    </div>
    <button
      class="tasks__card-button"
      @click="dragBacklogSprint"
      :disabled="!task.next_step_allowed"
    ><ArrowSvg /></button>
    <Modal 
        v-if="dragging && stage === 1" 
        :dragging="dragging" 
        :cancel="cancelDragging" 
        :task="task" 
        :finish="dragBacklogSprint"
        :toNext="toNext"
    />
    <EstimateModal 
        v-if="estimating && stage === 2" 
        :dragging="estimating" 
        :cancel="cancelDragging" 
        :task="task" 
        :finish="dragBacklogSprint"
        :toNext="toNext"
    />
  </div>
</template>

<script>
import Modal from "../Modal/Modal.vue";
import EstimateModal from "../Modal/EstimateModal.vue";
import Tag from "../Modal/Tag.vue";

import ArrowSvg from '@/assets/svg/arrow.svg'

export default {
  name: "Card",
  props: {
      task: {
          type: Object,
          default: () => {}
      },
      toNext: {
          type: Function
      },
      stage: {
          type: Number,
          default: 1
      }
  },
  components: {
    Modal,
    Tag,
    EstimateModal,
    ArrowSvg
  },
  data() {
    return {
      dragging: false,
      estimating: false
    };
  },
  methods: {
    dragBacklogSprint() {
      this.dragging = true
    },
    cancelDragging() {
        this.dragging = false
        this.estimating = false
    },
    addEstimation() {
        this.estimating = true
    }
  },
  created () {
    //   if (this.stage === 2) 
        // console.log(this.task)
  }
};
</script>

<style lang="scss">
    .tasks {
        &__card {
            display: flex;
            flex: 0 0 auto;
            padding: 20px;
            flex-direction: column;
            background: #fff;
            box-shadow: 0px 4px 10px rgba(170, 179, 188, 0.19);
            border-radius: 5px;
            margin-bottom: 20px;
            
            &.sprint {
                position: relative;

                &::before {
                    content: '';
                    top: 0;
                    left: 0;
                    position: absolute;
                    width: 6px;
                    height: 100%;
                    background-color: #e22d48;
                    border-top-left-radius: 5px;
                    border-bottom-left-radius: 5px;
                }
            }

            &-people {
                &--add {
                    width: 28px;
                    height: 28px;
                    border: 1px solid rgba(170, 179, 188, 0.31);
                    border-radius: 50%;
                    background-image: url('/images/svg/plus.svg');
                    background-repeat: no-repeat;
                    background-size: 8px;
                    background-position: center;
                    cursor: pointer;
                }
            }

            &-title {
                color: #2E2E2E;
                font-weight: 500;
                font-size: 14px;
                margin-bottom: 7px;
            }

            &-description {
                color: #AAB3BC;
                font-size: 12px;
                margin-bottom: 7px;
            }

            &-button {
                color: #fff;
                background: #51C75B;
                border-radius: 2px;
                padding: 4px 12px;
                align-self: flex-end;
                border: 0;

                &:disabled {
                    background: #AAB3BC;
                    cursor: not-allowed;
                }
            }
        }
    }

</style>

