<template>
  <div id="wapper">
    <el-form
      :model="dynamicValidateForm"
      ref="dynamicValidateForm"
      label-width="100px"
      class="demo-dynamic"
    >
      <el-form-item
        v-for="(item, index) in dynamicValidateForm.inputList"
        label=" "
        :key="item.key"
        :prop="'inputList.' + index + '.value'"
      >
        <el-date-picker
          :picker-options="pickerOptions"
          v-model="item.date"
          type="daterange"
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
        >
        </el-date-picker>
        <el-input
          @blur="validationNub(item.number)"
          v-model="item.number"
          placeholder="请输入数字"
        ></el-input>
      </el-form-item>
      <el-form-item>
        <el-button @click="addInputItem">新增</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      dynamicValidateForm: {
        inputList: [
          {
            date: "",
            number: ""
          }
        ]
      },
      pickerOptions: {
        disabledDate: time => {
          // console.log(this.dynamicValidateForm.inputList.map(item=>item.date));
        }
      }
    };
  },
  methods: {
    addInputItem() {
      if (this.validationNub(this.dynamicValidateForm.inputList[this.dynamicValidateForm.inputList.length-1].number)) {
				this.dynamicValidateForm.inputList.push({
					value: "",
					key: Date.now()
      	})
				return
      }
			this.$message.warning("数字不符合规则");
      return
    },
    addItem() {
      // let dom = `
      // 	<el-col :span="7">
      // 		<el-date-picker
      // 		v-model="value1"
      // 		type="daterange"
      // 		range-separator="至"
      // 		start-placeholder="开始日期"
      // 		end-placeholder="结束日期">
      // 		</el-date-picker>
      // 	</el-col>
      // 	<el-col :span="3" style="margin-left:1%">
      // 		<el-input placeholder="请输入数字" v-model="number1"/>
      // 	</el-col>
      // `
      // let dom1 = document.createElement('el-row')
      // dom1.innerHTML = dom
      // this.$refs.form.$el.append(dom1)
    },
    validationNub(num) {
      const reg = new RegExp("^-?\\d{0,9}$") ;
			return reg.test(num)
    }
  }
};
</script>
<style scoped>
/deep/ .el-input {
  width: 200px;
}
</style>
