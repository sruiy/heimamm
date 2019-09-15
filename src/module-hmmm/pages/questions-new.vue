<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-form :model="newForm" label-width="60px">
        <el-form-item label="学科 : ">
          <el-select v-model="newForm.subjectID" style="width:300px"></el-select>
        </el-form-item>
        <el-form-item label="目录 : ">
          <el-select v-model="newForm.catalogID" style="width:300px"></el-select>
        </el-form-item>
        <el-form-item label="企业 : ">
          <el-select v-model="newForm.enterpriseID" style="width:300px"></el-select>
        </el-form-item>
        <el-form-item label="城市 : ">
          <el-select v-model="newForm.city" style="width:145px"></el-select>
          <el-select v-model="newForm.province" style="width:150px"></el-select>
        </el-form-item>
        <el-form-item label="方向 : ">
          <el-select v-model="newForm.direction" style="width:300px"></el-select>
        </el-form-item>
        <el-form-item label="题型 : ">
          <el-radio-group v-model="newForm.questionType" @change="changeSelect">
            <el-radio :label="1+''">单选</el-radio>
            <el-radio :label="2+''">多选</el-radio>
            <el-radio :label="3+''">简答</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="难度 : ">
          <el-radio-group v-model="newForm.difficulty">
            <el-radio :label="1+''">简单</el-radio>
            <el-radio :label="2+''">一般</el-radio>
            <el-radio :label="3+''">困难</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="题干 : ">
         <el-input v-model="newForm.question" type="textarea" rows="5" style="width:800px"></el-input>
        </el-form-item>
        <el-form-item label="选项 : " v-if="allshow">
        <template v-if="selectshow">
          <el-radio v-model="isSelect" :label="0" style="margin-top:5px">A : <el-input v-model="newForm.options[0].title"></el-input></el-radio><br/>
          <el-radio v-model="isSelect" :label="1" style="margin-top:5px">B : <el-input v-model="newForm.options[1].title"></el-input></el-radio><br/>
          <el-radio v-model="isSelect" :label="2" style="margin-top:5px">C : <el-input v-model="newForm.options[2].title"></el-input></el-radio><br/>
          <el-radio v-model="isSelect" :label="3" style="margin-top:5px">D : <el-input v-model="newForm.options[3].title"></el-input></el-radio>
        </template>
        <template v-else>
         <el-checkbox v-model="newForm.options[0].isRight" :label="0" style="margin-top:5px">A : <el-input v-model="newForm.options[0].title"></el-input></el-checkbox><br/>
         <el-checkbox v-model="newForm.options[1].isRight" :label="1" style="margin-top:5px">B : <el-input v-model="newForm.options[1].title"></el-input></el-checkbox><br/>
         <el-checkbox v-model="newForm.options[2].isRight" :label="2" style="margin-top:5px">C : <el-input v-model="newForm.options[2].title"></el-input></el-checkbox><br/>
         <el-checkbox v-model="newForm.options[3].isRight" :label="3" style="margin-top:5px">D : <el-input v-model="newForm.options[3].title"></el-input></el-checkbox>
        </template>
        </el-form-item>

        <el-form-item label="答案 : ">
         <el-input v-model="newForm.answer" type="textarea" rows="5" style="width:800px"></el-input>
        </el-form-item>
        <el-form-item label="备注 : ">
         <el-input v-model="newForm.remarks" type="textarea" rows="5" style="width:800px"></el-input>
        </el-form-item>
        <el-form-item label="标签 : ">
         <el-input v-model="newForm.tags" style="width:300px"></el-input>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'QuestionsNew',
  data() {
    return {
      allshow: true,
      selectshow: true,
      isSelect: '',
      newForm: {
        subjectID: '',
        catalogID: '',
        enterpriseID: '',
        city: '',
        province: '',
        direction: '',
        questionType: '1',
        difficulty: '1',
        question: '',
        videoURL: 'http',
        answer: '',
        remarks: '',
        tags: '',
        options: [
          {code: 'A', title: '', img: '', isRight: false},
          {code: 'B', title: '', img: '', isRight: false},
          {code: 'C', title: '', img: '', isRight: false},
          {code: 'D', title: '', img: '', isRight: false}
        ]
      }
    }
  },
  watch: {
    isSelect: function (newVal) {
      for (let i = 0; i < this.newForm.options.length; i++) {
        this.newForm.options[i].isRight = false
      }
      this.newForm.options[newVal].isRight = true
    }
  },
  methods: {
    changeSelect(type) {
      if (type === '1') {
        this.allshow = true
        this.selectshow = true
      } else if (type === '2') {
        this.allshow = true
        this.selectshow = false
      } else {
        this.allshow = false
      }
    }
  }
}
</script>

<style scoped>
</style>
