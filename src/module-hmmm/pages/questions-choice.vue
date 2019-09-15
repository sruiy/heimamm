<template>
  <div>
    <el-row>
      <el-button size="mini" type="primary">新增试题</el-button>
      <el-button size="mini">批量导入</el-button>
    </el-row>
    <!--搜索条件-->
    <el-form :model="choiceForm">
      <el-row :gutter="5">
        <el-col :span="4">
          <el-form-item label="学科">
            <el-select v-model="choiceForm.subjectID" style="width:130px">
              <el-option
                v-for="item in subjectIDList"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="4">
          <el-form-item label="状态">
            <el-select v-model="choiceForm.publishType" style="width:130px">
              <el-option
                v-for="item in publishType"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="4">
          <el-form-item label="难度">
            <el-select v-model="choiceForm.difficulty" style="width:130px">
              <el-option
                v-for="item in difficulty"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="4">
          <el-form-item label="试题类型">
            <el-select v-model="choiceForm.questionType" style="width:100px">
              <el-option
                v-for="item in questionType"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="4">
          <el-form-item label="标签">
            <el-input v-model="choiceForm.tags" placeholder="请输入" style="width:130px"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="4">
          <el-form-item label="录入人">
            <el-input v-model="choiceForm.creatorID" placeholder="请输入" style="width:130px"></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row :gutter="5">
        <el-col :span="4">
          <el-form-item label="关键字">
            <el-input placeholder="请输入题目编号/题干" v-model="choiceForm.keyword" style="width:100px"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="4">
          <el-form-item label="题目备注">
            <el-input v-model="choiceForm.remarks" placeholder="请输入" style="width:100px"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="4">
          <el-form-item label="二级目录">
            <el-input v-model="choiceForm.catalogID" placeholder="请输入目录" style="width:100px"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="5">
          <el-form-item label="城市">
            <el-select v-model="choiceForm.province" style="width:90px" @change="getCitys">
              <el-option v-for="item in provinces" :key="item" :label="item" :value="item"></el-option>
            </el-select>
            <el-select v-model="choiceForm.city" style="width:90px">
              <el-option v-for="item in citys" :key="item" :label="item" :value="item"></el-option>
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="4">
          <el-form-item label="企业简称">
            <el-input v-model="choiceForm.shortName" placeholder="请输入" style="width:120px"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="3">
          <el-form-item label="方向">
            <el-input v-model="choiceForm.direction" placeholder="请输入" style="width:100px"></el-input>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-button size="mini" type="primary" @click="getQuestionsChoice()">搜索</el-button>
        <el-button size="mini" @click="clear">清除</el-button>
      </el-row>
    </el-form>
    <!--精选题库列表-->
    <el-tabs v-model="activeName" type="card" @tab-click="tabChange">
      <el-tab-pane label="全部" name="all">
        <el-table :data="questionsChoiceData" style="width: 100%" size="mini">
          <el-table-column type="index" label="序号"></el-table-column>
          <el-table-column prop="number" label="试题编号"></el-table-column>
          <el-table-column prop="subjectID" label="学科"></el-table-column>
          <el-table-column prop="questionType" label="题型" :formatter="getQuestionType"></el-table-column>
          <el-table-column prop="question" label="题干"></el-table-column>
          <el-table-column label="录入时间" width="100">
            <span slot-scope="obj">{{obj.row.addDate | parseTimeByString('{y}-{m}-{d}')}}</span>
          </el-table-column>
          <el-table-column prop="creator" label="录入人"></el-table-column>
          <el-table-column prop="difficulty" label="难度" :formatter="getDifficulty"></el-table-column>
          <el-table-column prop="id" label="使用次数"></el-table-column>
          <el-table-column prop="chkState" label="审核状态" :formatter="getChkState"></el-table-column>
          <el-table-column prop="chkRemarks" label="审核意见"></el-table-column>
          <el-table-column prop="chkUser" label="审核人"></el-table-column>
          <el-table-column prop="publishState" label="发布状态" :formatter="getPublishState"></el-table-column>
          <el-table-column label="操作" width="160">
            <template slot-scope="scope">
              <a href="javascript:;">审核</a>
              <a href="javascript:;">预览</a>
              <a href="javascript:;">下架</a>
              <a href="javascript:;">修改</a>
              <a href="javascript:;">删除</a>
            </template>
          </el-table-column>
        </el-table>
      </el-tab-pane>
      <el-tab-pane label="待审核" name="pending">
        <el-table :data="questionsChoiceData" style="width: 100%" size="mini">
          <el-table-column type="index" label="序号"></el-table-column>
          <el-table-column prop="number" label="试题编号"></el-table-column>
          <el-table-column prop="subjectID" label="学科"></el-table-column>
          <el-table-column prop="questionType" label="题型" :formatter="getQuestionType"></el-table-column>
          <el-table-column prop="question" label="题干"></el-table-column>
          <el-table-column label="录入时间" width="100">
            <span slot-scope="obj">{{obj.row.addDate | parseTimeByString('{y}-{m}-{d}')}}</span>
          </el-table-column>
          <el-table-column prop="creator" label="录入人"></el-table-column>
          <el-table-column prop="difficulty" label="难度" :formatter="getDifficulty"></el-table-column>
          <el-table-column prop="id" label="使用次数"></el-table-column>
          <el-table-column prop="chkState" label="审核状态" :formatter="getChkState"></el-table-column>
          <el-table-column prop="chkRemarks" label="审核意见"></el-table-column>
          <el-table-column prop="chkUser" label="审核人"></el-table-column>
          <el-table-column prop="publishState" label="发布状态" :formatter="getPublishState"></el-table-column>
          <el-table-column label="操作" width="160">
            <template slot-scope="scope">
              <a href="javascript:;">审核</a>
              <a href="javascript:;">预览</a>
              <a href="javascript:;">下架</a>
              <a href="javascript:;">修改</a>
              <a href="javascript:;">删除</a>
            </template>
          </el-table-column>
        </el-table>
      </el-tab-pane>
      <el-tab-pane label="已审核" name="solved">
        <el-table :data="questionsChoiceData" style="width: 100%" size="mini">
          <el-table-column type="index" label="序号"></el-table-column>
          <el-table-column prop="number" label="试题编号"></el-table-column>
          <el-table-column prop="subjectID" label="学科"></el-table-column>
          <el-table-column prop="questionType" label="题型" :formatter="getQuestionType"></el-table-column>
          <el-table-column prop="question" label="题干"></el-table-column>
          <el-table-column label="录入时间" width="100">
            <span slot-scope="obj">{{obj.row.addDate | parseTimeByString('{y}-{m}-{d}')}}</span>
          </el-table-column>
          <el-table-column prop="creator" label="录入人"></el-table-column>
          <el-table-column prop="difficulty" label="难度" :formatter="getDifficulty"></el-table-column>
          <el-table-column prop="id" label="使用次数"></el-table-column>
          <el-table-column prop="chkState" label="审核状态" :formatter="getChkState"></el-table-column>
          <el-table-column prop="chkRemarks" label="审核意见"></el-table-column>
          <el-table-column prop="chkUser" label="审核人"></el-table-column>
          <el-table-column prop="publishState" label="发布状态" :formatter="getPublishState"></el-table-column>
          <el-table-column label="操作" width="160">
            <template slot-scope="scope">
              <a href="javascript:;">审核</a>
              <a href="javascript:;">预览</a>
              <a href="javascript:;">下架</a>
              <a href="javascript:;">修改</a>
              <a href="javascript:;">删除</a>
            </template>
          </el-table-column>
        </el-table>
      </el-tab-pane>
    </el-tabs>
    <el-row type="flex" justify="center" style="margin: 20px 0">
      <el-pagination
        background
        :current-page="choiceForm.page"
        :page-size="choiceForm.pagesize"
        layout="prev, pager, next"
        :total="choiceForm.total"
        @current-change="currentChange"
      ></el-pagination>
    </el-row>
  </div>
</template>

<script>
import { simple as subjectSimple } from '@/api/hmmm/subjects'
import {
  chkType,
  publishType,
  difficulty,
  questionType
} from '@/api/hmmm/constants'
import { provinces, citys } from '@/api/hmmm/citys'
import { choice as questionsChoice, choicePublish } from '@/api/hmmm/questions'

export default {
  name: 'QuestionsChoice',
  data() {
    return {
      activeName: 'all',
      subjectIDList: [],
      publishType,
      difficulty,
      provinces,
      citys,
      questionType,
      chkType,

      questionsChoiceData: [
        {
          number: '',
          subjectID: '',
          questionType: '',
          question: '',
          addDate: '',
          creator: '',
          difficulty: '',
          id: '',
          chkState: '',
          chkRemarks: '',
          chkUser: '',
          publishState: ''
        }
      ],
      choiceForm: {
        subjectID: '',
        difficulty: '',
        questionType: '',
        tags: '',
        province: '',
        city: '',
        keyword: '',
        remarks: '',
        shortName: '',
        direction: '',
        creatorID: '',
        catalogID: '',
        chkState: '',
        publishType: '',
        page: 1,
        pagesize: 4,
        total: 0
      }
    }
  },
  methods: {
    tabChange(params) {
      this.choiceForm.page = 1
      this.change(params)
    },
    async change(params) {
      let actName = typeof params === 'string' ? params : params.name
      this.choiceForm.pagesize = actName === 'solved' ? 8 : 4

      if (actName === 'all') {
        await this.getQuestionsChoice()
      } else if (actName === 'pending') {
        await this.getQuestionsChoice()
        this.questionsChoiceData = this.questionsChoiceData.filter(item => {
          return item.chkState === 0
        })
      } else {
        await this.getQuestionsChoice(actName)
        this.questionsChoiceData = this.questionsChoiceData.filter(item => {
          return item.chkState === 1
        })
      }
    },
    currentChange(newPage) {
      this.choiceForm.page = newPage
      this.change(this.activeName)
    },
    clear() {
      for (let key in this.choiceForm) {
        this.choiceForm[key] = ''
        this.getQuestionsChoice()
      }
    },
    getPublishState(row, col, cellValue) {
      // debugger
      return cellValue === '' ? '待发布' : this.publishType[cellValue].label
    },
    getQuestionType(row, col, cellValue) {
      return cellValue ? this.questionType[cellValue - 1].label : '单选'
    },
    getDifficulty(row, col, cellValue) {
      return cellValue ? this.difficulty[cellValue - 1].label : '简单'
    },
    getChkState(row, col, cellValue) {
      return cellValue === '' ? '待审核' : this.chkType[cellValue].label
    },
    async getSubjectIDList() {
      let res = await subjectSimple()
      // console.log(res)
      this.subjectIDList = res.data
    },
    getCitys(pname) {
      this.choiceForm.city = ''
      this.citys = citys(pname)
    },
    async getQuestionsChoice() {

      let res = await questionsChoice(this.choiceForm)
      // console.log(res)
      this.questionsChoiceData = res.data.items
      this.choiceForm.total = res.data.counts
    }
  },
  created() {
    this.getSubjectIDList()
    this.provinces = provinces()
    this.getQuestionsChoice()
  }
}
</script>

<style scoped>
.el-tabs {
  margin: 10px 0 0 10px;
}
.el-form {
  margin-left: 10px;
}
.el-row {
  margin: 10px 0 0 20px;
}
</style>
