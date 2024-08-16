<template>
  <h1>{{ msg }}</h1>
  <div class="mt-4">
    <el-input
      v-model="inputPlay"
      style="max-width: 600px"
      placeholder="新增参赛选手"
      class="input-with-select"
    >
      <template #append>
        <el-button @click="addPlay">确认添加</el-button>
      </template>
    </el-input>
  </div>
  <div class="card">
      <el-transfer
      v-model="value"
      filterable
      style="width: 100%"
      filter-placeholder="搜索EG成员"
      :titles="['参赛名单', '晋级名单']"
      :data="transferData"
      
    />
    <el-divider />
    <el-button type="primary" round @click="getTeams">开始生成队伍配置</el-button>
    <el-table 
    :data="tableData" 
    :row-class-name="tableRowClassName"
    >
      <el-table-column prop="title" label="#" width="100" />
      <el-table-column prop="play1" label="队员1" width="180" />
      <el-table-column prop="play2" label="队员2" width="180" />
      <el-table-column prop="play3" label="队员3" />
    </el-table>
  </div>

  <p> 本次比赛感谢神总大力赞助和问天的技术支持！！！</p>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

defineProps<{ msg: string }>()
interface Option {
  key: number
  label: string
}
const plays = [
    'EG琴酒',
    'EG250',
    'EG小白',
    'EG胖虎',
    'EG梦魇',
    '老C',
    'T9',
    '大常',
    '热巴',
    '大鸡腿',
    '派大星',
    '青山',
    '钢琴',
    '大表哥',
    '鸭哥哥',
    '胖梨',
    '樱木',
    '高尔夫',
    '南柯',
    'EG问天',
    '全能野马',
  ]
const rules = [
  '双方其中一名成员必须使用洪寿',
  '所有人必须使用金币球员',
  '所有人不可使用橙卡',
  '所有人只能使用紫卡',
  '指定对面不得使用某2个球员',
  '指定一面必须使用某2个球员(不能指定金币卡)',
  '全员后卫',
]

const generateData = () => {
  const data: Option[] = []
  plays.forEach((city, index) => {
    data.push({
      label: city,
      key: index,
    })
  })
  return data
}
const tableRowClassName = ({
  row,
  rowIndex,
}: {
  row: any
  rowIndex: number
}) => {
  console.log(row)
  if (rowIndex <=2 || (rowIndex>5&&rowIndex<=8)|| (rowIndex>11&&rowIndex<=14)|| (rowIndex>17&&rowIndex<=20)) {
    return 'success-row'
  }else {
    return 'warning-row'
  }
  return ''
}

const inputPlay = ref()
const addPlay = () => {
  plays.splice(0, 0, inputPlay.value);
  transferData.value = generateData()
}
const value = ref([])
const transferData = ref<Option[]>(generateData())

const tableData = ref<{ title: string;play1: string; play2: string; play3: string }[]>([]);

const getTeams = () => {
  
  // 创建一个新的数组副本
  const shuffledData = [...value.value];

  // 随机排列数组元素
  shuffledData.sort(() => Math.random() - 0.5);

  // 将数据划分为每三个一组的数组
  tableData.value = [];
  for (let i = 0; i < shuffledData.length; i += 3) {
    tableData.value.push({
      title: "参赛队伍",
      play1: plays[shuffledData[i]],
      play2: plays[shuffledData[i + 1]],
      play3: plays[shuffledData[i + 2]],
    });
    if (i ==3||i==9||i==15||i==21||i==27||i==33) {

    rules.sort(() => Math.random() - 0.5);
    tableData.value.push({
      title: "趣味规则",
      play1: rules[0],
      play2: rules[1],
      play3: rules[2],
    });
    }

  }
};

</script>

<style lang="scss" scoped>
.read-the-docs {
  color: #888;
}

.input-with-select .el-input-group__prepend {
  background-color: var(--el-fill-color-blank);
}
.el-table {
    :deep(.warning-row){
      --el-table-tr-bg-color: var(--el-color-warning-light-9);
    }
}
.el-table {
    :deep(.success-row){
      --el-table-tr-bg-color: var(--el-color-success-light-9);
    }
}

</style>
