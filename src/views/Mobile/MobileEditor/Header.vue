<template>
  <div class="mobile-editor-header">
    <div class="history">
      <div class="history-item" :class="{ 'disable': !canUndo }" @click.stop="undo()"><IconBack /> 撤销</div>
      <div class="history-item" :class="{ 'disable': !canRedo }" @click.stop="redo()"><IconNext /> 重做</div>
    </div>
    <div class="back" @click="changeMode('preview')"><IconLogout /> 退出编辑</div>
  </div>
</template>

<script lang="ts" setup>
import { PropType } from 'vue'
import { storeToRefs } from 'pinia'
import { useSnapshotStore } from '@/store'
import { Mode } from '@/types/mobile'
import useHistorySnapshot from '@/hooks/useHistorySnapshot'

defineProps({
  changeMode: {
    type: Function as PropType<(mode: Mode) => void>,
    required: true,
  },
})

const { canUndo, canRedo } = storeToRefs(useSnapshotStore())
const { redo, undo } = useHistorySnapshot()
</script>

<style lang="scss" scoped>
.mobile-editor-header {
  height: 50px;
  background-color: #fff;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 18px;
  font-size: 13px;
  box-shadow: 0 0 15px 0 rgba(0, 0, 0, 0.1);
}
.history {
  display: flex;
  justify-content: center;
  align-items: center;
}
.history-item {
  margin-right: 20px;

  &.disable {
    opacity: .5;
  }
}
</style>