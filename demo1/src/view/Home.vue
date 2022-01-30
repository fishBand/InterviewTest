<template>	
  <div id="wapper">
		<el-form ref="form">
			<el-row :gutter="15">
				<el-col :span="7">
					<el-date-picker
					v-model="value1"
					type="daterange"
					range-separator="至"
					start-placeholder="开始日期"
					end-placeholder="结束日期">
					</el-date-picker>
				</el-col>
				<el-col :span="3" style="margin-left:1%">
					<el-input placeholder="请输入数字" v-model="number1"/>
				</el-col>
			</el-row>
		</el-form>
		<el-button @click="addItem" type="primary" size="mini" style="margin-top:1%">新增</el-button>
  </div>
</template>
    
<script>
export default {
    name: 'Home',
		data(){
			return{
				value1:'',
				number1:''
			}
		},
		methods:{
			addItem(){
				let dom = `
					<el-col :span="7">
						<el-date-picker
						v-model="value1"
						type="daterange"
						range-separator="至"
						start-placeholder="开始日期"
						end-placeholder="结束日期">
						</el-date-picker>
					</el-col>
					<el-col :span="3" style="margin-left:1%">
						<el-input placeholder="请输入数字" v-model="number1"/>
					</el-col>
				`
				let dom1 = document.createElement('div')
				dom1.setAttribute('class','el-row')
				dom1.innerHTML = dom
				this.$refs.form.$el.append(dom1)
			}
		}
}
</script>