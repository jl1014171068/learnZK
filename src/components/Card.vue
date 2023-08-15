<script setup lang="js" name="Card">
import { ref, computed, useSlots } from 'vue'
import { cloneDeep } from 'lodash'
const props = defineProps({
  result: {
    type: Object,
    default: () => ({})
  }
})
const data = ref(props.result.list)

const handleSelectRadio = (val,index)=>{
  const {value} = data['value'][index]
  if(value !== val) {
    data['value'][index].isYes = false
    return
  }
  data['value'][index].isYes = true
}
const handleShow = ()=>{
  data.value.forEach(v=>{
    v.isShowResult = true
  })
}
</script>
<template>
  <div class="card">
    <div class="second-title" @click="handleShow">
      {{ result.title }}
    </div>
    <div class="box">
      <template v-for="(list, index) in data" :key="index">
        <div class="label">
          {{ index + 1 }}：{{ list.title }}
          <template v-if="list.isShowResult">
            <span class="success" v-if="['select'].includes(list.itemType)">{{
              list.value
            }}</span>
            <span class="success" v-if="['checkbox'].includes(list.itemType)">{{
              list.value ? '是' : '否'
            }}</span>
          </template>
          <template v-if="list.isYes !== null">
            <span v-if="list.isYes" class="success">答对了</span>
            <span v-else class="error">答错了，答案为：{{ list.value }}</span>
          </template>
        </div>
        <div class="value">
          <template v-if="['select'].includes(list.itemType)">
            <el-radio-group
              v-model="list.model"
              class="ml-4"
              @change="(val) => handleSelectRadio(val, index)"
            >
              <el-radio
                v-for="(l, i) in list.options"
                :key="`${index}_radio_${i}_${list.title}`"
                :label="l.value"
                size="large"
              >
                {{ l.value }}：{{ l.label }}</el-radio
              >
            </el-radio-group>
          </template>
          <!-- <ul v-if="['select'].includes(list.itemType)">
            <li v-for="(ll, ii) in list.options" :key="`${index}_${ii}`">
              {{ ll.value }}：{{ ll.label }}
            </li>
          </ul> -->
          <p v-else>答案：{{ list.value }}</p>
        </div>
      </template>
    </div>
  </div>
</template>

<style scoped lang="scss">
.card {
  .success {
    color: green;
  }
  .error {
    color: red;
  }
  ul,
  li {
    list-style: none;
    margin: 0;
    padding: 0;
  }
  .second-title {
    font-weight: bold;
    color: #303133;
    font-size: 16px;
    max-width: 100%;
  }

  .box {
    .value {
      margin-left: 20px;
    }
    ul {
      li {
      }
    }
  }
  .label {
  }
}
</style>
