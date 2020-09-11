<template>
  <div id="vm">
    <el-form ref="ruleForm" :model="ruleForm" :rules="rules" label-width="100px" class="demo-ruleForm" style="margin:16px;width:60%">
      <!-- 问题简述 -->
      <el-form-item label="问题描述" prop="question">
        <el-input v-model="ruleForm.question" placeholder="说说你遇到的问题" minlength="3" maxlength="40" show-word-limit />
      </el-form-item>
      <!--  -->

      <!-- 标签 -->
      <el-form-item label="问题标签" prop="tag">
        <el-checkbox-group v-model="ruleForm.tag" :max="4">
          <el-checkbox-button v-for="tag in tags" :key="tag" :label="tag" style="margin:4px">{{ tag }}</el-checkbox-button>
        </el-checkbox-group>
      </el-form-item>
      <!--  -->

      <!-- 报错类型，必填 -->
      <!-- <el-form-item label="问题标签" prop="tag">
        <el-select
          v-model="value"
          multiple
          filterable
          allow-create
          default-first-option
          placeholder="出现了什么情况？可以自己编辑！"
        >
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item> -->
      <!--  -->

      <!-- 上传文件 -->
      <!-- <p style="font-family: Arial, Helvetica, sans-serif;font-size:15px;margin:16px">上传文件</p>
      <el-upload
        class="upload-demo"
        action="https://jsonplaceholder.typicode.com/posts/"
        :on-preview="handlePreview"
        :on-remove="handleRemove"
        :before-remove="beforeRemove"
        multiple
        :limit="5"
        :on-exceed="handleExceed"
        :file-list="fileList"
        style="margin:16px"
      >
        <el-button size="small" type="primary" icon="el-icon-upload">点击上传文件</el-button>
        <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
      </el-upload>-->
      <!--  -->

      <!-- 问题细节，后期利用富文本实现 -->
      <el-form-item label="相关细节" prop="detail">
        <el-input
          v-model="ruleForm.detail"
          :autosize="{ minRows: 4, maxRows:10 }"
          type="textarea"
          placeholder="问题有哪些细节？写在这里！（比如说:问题部分的源代码和相关的报错信息！）"
          maxlength="1200"
          show-word-limit
        />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')">提交</el-button>
        <el-button @click="resetForm('ruleForm')">重置</el-button>
      </el-form-item>
      <!--  -->
    </el-form>
  </div>
</template>
<script>
export default {
  data() {
    return {
      tags: ['wxml', 'js', 'wxss', 'json', '云函数', '路径', '没有报错', '变量', 'undefined'],
      ruleForm: {
        question: '',
        tag: [],
        detail: ''
      },
      rules: {
        question: [
          { required: true, message: '先说想问的问题吧', trigger: 'blur' },
          { min: 3, max: 40, message: '简述的字数应该在3~40之间哟', trigger: 'blur' }
        ],
        detail: [
          { max: 1200, message: '字数不能超过300字', trigger: 'blur' }
        ]
      },
      fileList: []
    }
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert('你的问题已经提交了!你的问题很快会被解答哟')
          // 提交成功后，清空文本
          this.ruleForm.question = ''
          while (this.ruleForm.tag.length !== 0) {
            this.ruleForm.tag.pop()
          }
          this.ruleForm.detail = ''
        } else {
          console.log('系统繁忙，提交失败了')
          return false
        }
      })
    },
    resetForm(formName) {
      this.$refs[formName].resetFields()
    },
    handleRemove(file, fileList) {
      console.log(file, fileList)
    },
    handlePreview(file) {
      console.log(file)
    },
    handleExceed(files, fileList) {
      this.$message.warning(`当前限制选择 5 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`)
    },
    beforeRemove(file, fileList) {
      return this.$confirm(`确定移除 $ { file.name }？`)
    }
  }
}
</script>
