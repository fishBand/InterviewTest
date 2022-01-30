<template>
  <div id="wapper">
    <a-table :columns="columns" :data-source="data">
      <template slot="action" slot-scope="text, record">
        <a slot="action" href="javascript:;" @click="edit(record)">编辑</a>
      </template>
    </a-table>
    <a-modal v-model="isShowEdit" title="编辑" @ok="sub">
      <a-form :form="entityForm" :label-col="{ span: 5 }" :wrapper-col="{ span: 12 }" >
        <a-form-item label="字段名称">
          <a-input v-model="entityFormData.name"></a-input>
        </a-form-item>
        <a-form-item label="字段类型">
          <a-select v-model="entityFormData.type" default-value="日期" @change="onChangeType">
            <a-select-option value="日期">
              日期
            </a-select-option>
            <a-select-option value="单行文本">
              单行文本
            </a-select-option>
            <a-select-option value="单选下拉">
              单选下拉
            </a-select-option>
          </a-select>
        </a-form-item>
        <a-form-item label="日期类型" v-if="isShowDateOption">
          <a-radio-group v-model="entityFormData.option" :options="plainOptions" default-value="年 - 月" @change="changeDateOption" />
        </a-form-item>
        <a-form-item label="选项" v-if="isOptions">
          <template >
            <a-input  v-for="item in optionList" :key="item" :value="item" />
          </template>
          <a-button @click="addOptionItem" type="primary">增加</a-button>
          <a-button @click="optionList.pop()" type="danger">删除</a-button>
        </a-form-item>
        <a-form-item label="是否必填">
          <a-select v-model="entityFormData.required"  @change="onChangeRequire">
            <a-select-option value="是">
              是
            </a-select-option>
            <a-select-option value="否">
              否
            </a-select-option>
          </a-select>
        </a-form-item>
      </a-form>
    </a-modal>
  </div>
</template>

<script>
const columns = [
  {
    title: "字段名称",
    dataIndex: "name",
    key: "name"
  },
  {
    title: "字段类型",
    dataIndex: "type",
    key: "type"
  },
  {
    title: "是否必填",
    dataIndex: "required",
    key: "required"
  },
  {
    title: "字段选项",
    key: "option",
    dataIndex: "option"
  },
  {
    title: "操作",
    key: "action",
    dataIndex: "action",
    scopedSlots: { customRender: "action" }
  }
];
const data = [
  {
    key: "1",
    name: "字段一",
    type: "单行文本",
    required: "是",
    option: "无"
  },
  {
    key: "2",
    name: "字段二",
    type: "日期",
    required: "否",
    option: "年 - 月 - 日"
  },
  {
    key: "3",
    name: "字段三",
    type: "单选下拉",
    required: "否",
    option: ["选项一","选项二"]
  }
];
export default {
  name: "Home",
  data() {
    return {
      data,
      columns,
      isShowEdit:false,
      isShowDateOption:true,
      isOptions:false,
      optionList:[],
      entityForm: this.$form.createForm(this, { name: 'coordinated' }),
      entityFormData:{},
      plainOptions:['年 - 月', '年 - 月 - 日', '年 - 月 - 日 - 时 - 分']
    };
  },
  methods: {
    edit(value) {
      this.entityFormData = JSON.parse(JSON.stringify(value))
      this.isShowEdit = true
      
      if(this.entityFormData.type == '日期') {
        this.isShowDateOption = true
        this.isOptions = false
      }
      if(this.entityFormData.type == '单行文本') {
        this.isShowDateOption = false
        this.isOptions = false
        this.entityFormData.option = '无'
      }
      if(this.entityFormData.type == '单选下拉') {
        this.isShowDateOption = false
        this.isOptions = true
        this.optionList = this.entityFormData.option
      }
    },
    sub() {
      this.isShowEdit = false
      this.data.forEach( (item,index) => {
        if(item.key == this.entityFormData.key){
          this.$set(this.data, index, this.entityFormData)
          this.data[index] = this.entityFormData
        }
      })
    },
    onChangeType(e) {
      this.entityFormData.type = e
      if(e == '日期') {
        this.isShowDateOption = true
        this.isOptions = false
      }
      if(e == '单行文本') {
        this.isShowDateOption = false
        this.entityFormData.option = '无'
        this.isOptions = false
      }
      if(e == '单选下拉') {
        this.isShowDateOption = false
        this.isOptions = true
      }
    },
    changeDateOption(e) {
      this.entityFormData.option = e.target.value
    },
    onChangeRequire(e) {
      this.entityFormData.required = e
    },
    addOptionItem() {
      this.optionList.push('')
    }
  }
};
</script>
