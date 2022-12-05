<template>
  <div class="container">
    <el-card style="margin-bottom: 10px; margin-top: 10px">
      <!-- 上方说明 和 新增试题按钮 -->
      <el-row type="flex" justify="space-between" style="margin-bottom: 10px">
        <span class="shuoming">说明：目前支持学科和关键字条件筛选</span>
        <el-button type="success" round @click="$router.push('/questions/new')">
          <i class="el-icon-edit"></i>新增试题</el-button
        >
      </el-row>
      <!-- 上方学科 筛选 表单 -->
      <el-form :model="searchform" ref="searchform" :rules="rules">
        <el-row :gutter="20">
          <el-col :span="6">
            <el-form-item
              label="学科"
              style="margin-left: 20px"
              prop="subjectID"
            >
              <el-select
                v-model="searchform.subjectID"
                placeholder="请选择"
                style="width: 165px"
                @change="change"
              >
                <el-option
                  :label="obj.subjectName"
                  :value="obj.id"
                  v-for="obj in simplelist"
                  :key="obj.id"
                ></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="试题类型">
              <el-select
                v-model="searchform.questionType"
                placeholder="请选择"
                style="width: 165px"
              >
                <el-option
                  :label="obj.label"
                  :value="obj.value"
                  v-for="(obj, index) in questionType"
                  :key="index"
                ></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="题目备注">
              <el-input
                v-model="searchform.remarks"
                placeholder=""
                style="width: 165px"
              ></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="二级目录">
              <el-select
                v-model="searchform.catalogID"
                placeholder="请选择"
                :span="6"
                style="width: 165px"
              >
                <el-option
                  :label="obj.directoryName"
                  :value="obj.id"
                  v-for="obj in twocatlist"
                  :key="obj.id"
                ></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="难度" style="margin-left: 30px">
              <el-select
                v-model="searchform.difficulty"
                placeholder="请选择"
                :span="6"
                style="width: 165px"
              >
                <el-option
                  :label="obj.label"
                  :value="obj.value"
                  v-for="(obj, index) in difficulty"
                  :key="index"
                ></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="企业简称">
              <el-input
                v-model="searchform.shortName"
                style="width: 165px"
              ></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="标签">
              <el-select
                v-model="searchform.tags"
                placeholder="请选择"
                style="width: 165px"
              >
                <el-option
                  :label="obj.tagName"
                  :value="obj.id"
                  v-for="obj in labellist"
                  :key="obj.id"
                ></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="方向">
              <el-select
                v-model="searchform.direction"
                placeholder="请选择"
                style="width: 165px"
              >
                <el-option
                  :label="str"
                  :value="str"
                  v-for="(str, index) in direction"
                  :key="index"
                ></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="城市">
              <el-select
                v-model="searchform.city"
                placeholder="请选择"
                style="width: 82px"
                @change="changecity"
              >
                <el-option
                  :label="str"
                  :value="str"
                  v-for="(str, index) in provinces()"
                  :key="index"
                ></el-option>
              </el-select>
              <el-select
                v-model="searchform.province"
                placeholder="请选择"
                style="width: 82px; margin-left: 5px"
              >
                <el-option
                  :label="str"
                  :value="str"
                  v-for="(str, index) in citylist"
                  :key="index"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <el-form-item label="关键字">
              <el-input
                v-model="searchform.keyword"
                placeholder="根据题干搜索"
                style="width: 165px"
              ></el-input>
            </el-form-item>
            <el-form-item label="录入人">
              <el-select
                v-model="searchform.creatorID"
                placeholder="请选择"
                style="width: 165px"
              >
                <el-option
                  :label="obj.username"
                  :value="obj.id"
                  v-for="obj in userlist1"
                  :key="obj.id"
                ></el-option>
              </el-select>
            </el-form-item>
            <el-form-item style="margin-left: 100px">
              <el-button size="small" @click="restform">清除</el-button>
              <el-button size="small" type="primary" @click="okSearch"
                >搜索</el-button
              >
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <el-row class="number">
        <i class="el-icon-warning"></i>&nbsp;数据一共 {{ counts }} 条</el-row
      >
      <!-- 下方表格 -->
      <el-table :data="tableData" style="width: 100%">
        <el-table-column prop="number" label="试题编号" width="120">
        </el-table-column>
        <el-table-column prop="subject" label="学科" width="80">
        </el-table-column>
        <el-table-column prop="catalog" label="目录" width="80">
        </el-table-column>
        <el-table-column
          prop="questionType"
          label="题型"
          width="280"
          :formatter="formattype"
        >
        </el-table-column>
        <el-table-column
          prop="question"
          label="题干"
          width="300"
          :formatter="formatcontext"
        >
        </el-table-column>
        <el-table-column
          prop="addDate"
          label="录入时间"
          width="180"
          :formatter="formattertime"
        >
        </el-table-column>
        <el-table-column
          prop="difficulty"
          label="难度"
          width="70"
          :formatter="formatdifficulty"
        >
        </el-table-column>
        <el-table-column prop="creator" label="录入人" width="90">
        </el-table-column>
        <el-table-column label="操作" width="180">
          <template slot-scope="scope">
            <el-button
              type="primary"
              icon="el-icon-view"
              circle
              size="small "
              title="预览"
              @click="preview(scope.row)"
            ></el-button>
            <el-button
              type="success"
              icon="el-icon-edit"
              circle
              size="small"
              title="修改"
              @click="$router.push(`/questions/new/?id=${scope.row.id}`)"
            ></el-button>
            <el-button
              type="danger"
              icon="el-icon-delete"
              circle
              size="small "
              title="删除"
              @click="deletequestion(scope.row)"
            ></el-button>
            <el-button
              type="warning"
              icon="el-icon-check"
              circle
              size="small "
              title="加入精选"
              @click="addjingxuan(scope.row)"
            ></el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-row type="flex" justify="end" style="margin-top: 10px">
        <span
          ><el-pagination
            background
            layout="sizes,prev, pager, next,jumper"
            :total="counts"
            :page-sizes="[5, 10, 20, 50]"
            :page-size="searchform.pagesize"
            @size-change="changepageSize"
            @current-change="currentchange"
          >
          </el-pagination
        ></span>
      </el-row>
    </el-card>
    <!-- 预览层 -->
    <el-dialog
      title="题目预览"
      :visible.sync="dialogVisible"
      width="70%"
      :destroy-on-close="true"
      @close="beforeclosedialog"
    >
      <span class="preview">
        <el-row type="flex">
          <el-col style="margin-left: 10px">
            <span
              >【题型】：{{
                thispreviewrow.questionType | getquestinostype123
              }}题</span
            >
            <span>【学科】：{{ thispreviewrow.subjectName }}</span>
          </el-col>
          <el-col>
            <span>【编号】：{{ thispreviewrow.id }}</span>
            <span>【目录】：{{ thispreviewrow.directoryName }}</span>
          </el-col>
          <el-col>
            <span
              >【难度】：{{ thispreviewrow.difficulty | getdifficult }}</span
            >
            <span>【方向】：{{ thispreviewrow.direction }}</span>
          </el-col>
          <el-col>
            <span>【标签】：{{ thispreviewrow.tags }}</span>
          </el-col>
        </el-row>
        <hr />
        <div style="margin-left: 19px">
          <span>【题干】：</span>
          <div style="margin: 10px; color: #3a3aff">
            {{ thispreviewrow.question | gethtmlcontext }}
          </div>
          <span
            >{{ thispreviewrow.questionType | getquestinostype123 }}题
            选项：（以下选中的选项为正确答案）</span
          >
          <!-- 单选组 -->
          <el-radio-group
            v-if="thispreviewrow.questionType == 1"
            v-model="radio"
            style="display: block; margin-top: 20px"
          >
            <el-radio
              :label="obj.title"
              v-for="obj in thispreviewrow.options"
              :key="obj.id"
              :selected="obj.isRight === 1"
              >{{ obj.title }}</el-radio
            >
          </el-radio-group>
          <!-- 多选组 -->
          <el-checkbox-group
            v-model="checkList"
            v-if="thispreviewrow.questionType == 2"
            style="margin-top: 20px"
          >
            <el-checkbox
              :label="obj.title"
              v-for="obj in optionslist"
              :key="obj.id"
              :checked="Boolean(obj.isRight)"
            ></el-checkbox>
          </el-checkbox-group>
        </div>
        <hr />
        <div style="margin-left: 19px">
          <span>【参考答案】：</span>
          <el-button type="danger" @click="showvideo = true"
            >视频答案预览</el-button
          >
          <video
            v-if="showvideo"
            :src="thispreviewrow.videoURL"
            controls
            controlslist="nodownload"
            width="400"
            height="300"
            style="display: block; margin-top: 20px"
          ></video>
        </div>
        <hr />
        <div style="margin-left: 19px; margin-top: 20px; margin-bottom: 10px">
          <span>【答案解析】：</span>
          <span>{{ thispreviewrow.answer | gethtmlcontext }}</span>
        </div>
        <hr />
        <div style="margin-left: 19px">
          <span>【题目备注】：</span>
          <span>{{ thispreviewrow.remarks }}</span>
        </div>
      </span>

      <el-row type="flex" justify="end"
        ><el-button type="primary" @click="dialogVisible = false"
          >关闭</el-button
        ></el-row
      >
    </el-dialog>
  </div>
</template>

<script>
import { list } from "../../api/hmmm/subjects";
import { list as directoryslist } from "../../api/hmmm/directorys";
import { list as tagslist } from "../../api/hmmm/tags";
import { list as userlist } from "../../api/base/users";
import { questionType, difficulty, direction } from "../../api/hmmm/constants";
import { provinces, citys } from "../../api/hmmm/citys";
import {
  list as questionslist,
  remove,
  choiceAdd,
  detail,
} from "../../api/hmmm/questions";
import { parseTimeByString, html2Text } from "../../filters";
export default {
  name: "questions",
  filters: {
    getquestinostype123(value) {
      let b = "";
      questionType.map((obj) => {
        if (obj.value == value) {
          b = obj.label;
        }
      });
      return b;
    },
    getdifficult(value) {
      let a = "";
      difficulty.map((obj) => {
        if (obj.value == value) {
          a = obj.label;
        }
      });
      return a;
    },
    gethtmlcontext(value) {
      return html2Text(value);
    },
  },
  data() {
    return {
      thispreviewrow: {},
      checkList: [], ///多选 选中存放
      optionslist: [], ///多选遍历选项
      optionlist: [], ///单选遍历选项
      showvideo: false,
      radio: "", ///单选选中存放
      dialogVisible: false,
      simplelist: [],
      twocatlist: [],
      labellist: [],
      userlist1: [],
      questionType: questionType,
      difficulty: difficulty,
      direction: direction,
      provinces: provinces,
      citylist: [],
      searchform: {
        subjectID: "", //学科
        questionType: "", //试题类型
        remarks: "", //题目备注
        catalogID: "", //二级目录
        difficulty: "", //难度
        shortName: "", //企业简称
        tags: "", //标签
        direction: "", //方向
        city: "", //企业所在城市
        province: "", //企业所在地省份
        keyword: "", //关键词
        creatorID: "", //录入人
        page: 1,
        pagesize: 5,
      },
      rules: {
        subjectID: [
          { required: true, message: "学科不能为空呐", trigger: "blur" },
        ],
      },
      tableData: [],
      counts: 0,
    };
  },

  methods: {
    beforeclosedialog() {
      this.showvideo = false;
    },
    getradiostatus() {
      const res = this.thispreviewrow.options.filter(
        (obj) => obj.isRight === 1
      );
      ///因为这里是单选嘛 ，所以将 这个当中的 title添加到数组中就可以了
      console.log(res);
      this.radio = res[0].title;
      console.log(this.radio);
    },
    async preview(row) {
      const { data } = await detail({ id: row.id });
      this.thispreviewrow = data;
      this.getradiostatus(); //调用 单选方法
      this.optionslist = data.options;

      this.dialogVisible = true;
    },
    async addjingxuan(row) {
      await this.$confirm("确定要加入精选吗？", "精选", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      });
      await choiceAdd({ id: row.id, choiceState: 1 });
      this.$message({
        type: "success",
        message: "加入精选成功!",
      });
      this.currentchange();
    },
    async deletequestion(row) {
      await this.$confirm("此操作将永久删除该数据, 是否继续?", "删除", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      });
      await remove(row.id); /////需要穿id
      this.$message({
        type: "success",
        message: "删除成功!",
      });
      this.currentchange();
    },
    currentchange(currentpage) {
      this.searchform.pagesize = currentpage;
      if (this.searchform.subjectID != "") {
        this.gettablelist(this.searchform);
      } else {
        this.gettablelist({ pagesize: this.searchform.pagesize });
      }
    },
    changepageSize(pagesize) {
      this.searchform.pagesize = pagesize;
      if (this.searchform.subjectID != "") {
        this.gettablelist(this.searchform);
      } else {
        this.gettablelist({ pagesize: this.searchform.pagesize });
      }
    },
    formatdifficulty(row, column, cellValue, index) {
      let a = "";
      difficulty.map((obj) => {
        if (obj.value == cellValue) {
          a = obj.label;
        }
      });
      return a;
    },
    formattype(row, column, cellValue, index) {
      let a = "";
      questionType.map((obj) => {
        if (obj.value == cellValue) {
          a = obj.label;
        }
      });

      return a;
    },
    formatcontext(row, column, cellValue, index) {
      return html2Text(cellValue);
    },
    formattertime(row, column, cellValue, index) {
      return parseTimeByString(cellValue);
    },
    async okSearch() {
      await this.$refs.searchform.validate();
      this.gettablelist(this.searchform);
    },
    async gettablelist(a) {
      const { data } = await questionslist(a);
      this.counts = data.counts;
      if (data.items && data.items.length != 0) {
        this.tableData = data.items;
      } else {
        this.tableData = [];
      }
    },
    changecity(value) {
      this.citylist = citys(value);
    },
    async userlist() {
      const { data } = await userlist({ pagesize: 1000 });
      this.userlist1 = data.list;
    },
    async change(value) {
      // 根据学科 id 获取 相应 二级目录
      const { data } = await directoryslist({
        pagesize: 1000,
        subjectID: value,
      });
      if (data.items && data.items.length != 0) {
        this.twocatlist = data.items;
      }
      // 根据学科id 获取标签
      const res = await tagslist({ pagesize: 1000, subjectID: value });
      this.labellist = res.data.items;
    },
    // 获取列表
    async simple() {
      const { data } = await list({ pagesize: 1000 });
      this.simplelist = data.items;
    },
    restform() {
      this.searchform = {};
    },
  },
  created() {
    this.simple(); //获取学科 简单列表
    this.userlist(); ///获取用户列表
    this.gettablelist({ pagesize: this.searchform.pagesize }); ///拿到 题库数据
  },
};
</script>

<style scoped lang='scss'>
.preview .el-radio,
.el-checkbox {
  display: block;
  margin-bottom: 10px;
}

.preview .el-col {
  display: flex;
  flex-direction: column;
}
.preview .el-col span {
  margin: 10px;
}
::v-deep .shuoming {
  color: red;
}

.number {
  height: 35px;
  margin-bottom: 15px;
  margin-top: 15px;
  background-color: rgb(244, 244, 245);
  font-size: 15px;
  line-height: 35px;
  color: rgb(144, 147, 153);
  padding-left: 10px;
}
</style>
