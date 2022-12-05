<template>
  <div class='container'>
    <!-- 新增 -->
    <el-row type="flex" class="row-bg" justify="space-between" align="middle">
      <el-col :span="6">
        <div class="grid-content bg-purple">说明：目前支持学科和关键字条件筛选</div>
      </el-col>
      <el-col :span="6">
        <div class="grid-content bg-purple-light">
        <el-row  type="flex" justify="end" ><el-button type="success"  icon="el-icon-edit" size="small">新增试题</el-button></el-row>
        </div>
      </el-col>  
    </el-row>
    <!-- 表单信息 -->
    <el-form :inline="true"  class="demo-form-inline" :label-position="labelPosition" :model="formdata" :rules="rules">
      <el-row :gutter="20">
        <el-col :span="6">
          <div class="grid-content bg-purple">
            <el-form-item label="学科" prop="subject" class="subject">
            <el-select placeholder="请选则" :v-model="formdata.subject">
              <el-option label="区域一"></el-option>
            </el-select>
          </el-form-item></div>
        </el-col>
        <el-col :span="6">
          <div class="grid-content bg-purple">
            <el-form-item label="二级目录" prop="seconditem">
            <el-select placeholder="请选则" :v-model="formdata.seconditem">
              <el-option label="区域一" value="11"></el-option>
            </el-select>
          </el-form-item></div>
        </el-col>
        <el-col :span="6">
          <div class="grid-content bg-purple">
              <el-form-item label="标签" prop="tags">
                <el-select placeholder="请选则" :v-model="formdata.tags">
                  <el-option label="区域一" value="11"></el-option>
                </el-select>
              </el-form-item>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="grid-content bg-purple">
              <el-form-item label="关键字" prop="keywords">
                <el-input placeholder="请选则" :v-model="formdata.keywords"></el-input>
              </el-form-item>
          </div>
        </el-col>
      </el-row>
      <el-row :gutter="20">
        <el-col :span="6">
          <div class="grid-content bg-purple">
              <el-form-item label="试题类型" prop="type">
                <el-select placeholder="请选则" :v-model="formdata.type">
                  <el-option label="区域一" value="11"></el-option>
                </el-select>
              </el-form-item>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="grid-content bg-purple">
            <el-form-item label="难度" prop="difficulty" class="difficulty">
              <el-select placeholder="请选则" :v-model="formdata.difficulty">
                <el-option label="区域一" value="111"></el-option>
              </el-select>
            </el-form-item>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="grid-content bg-purple">
              <el-form-item label="方向" prop="direction">
                <el-select placeholder="请选则" :v-model="formdata.direction">
                  <el-option label="区域一" value="111"></el-option>
                </el-select>
              </el-form-item>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="grid-content bg-purple">
            <el-form-item label="录入人" prop="inputpeople">
              <el-select placeholder="请选则" :v-model="formdata.inputpeople">
                <el-option label="区域一" value="111"></el-option>
              </el-select>
            </el-form-item>
          </div>
        </el-col>
      </el-row>
      <el-row :gutter="20">
        <el-col :span="6">
          <div class="grid-content bg-purple">
              <el-form-item label="备注" prop="remark" class="remark">
                <el-input placeholder="请选则" :v-model="formdata.remark"></el-input>
              </el-form-item>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="grid-content bg-purple">
                <el-form-item label="企业简称" prop="companyname">
                  <el-input placeholder="请选则" :v-model="formdata.companyname"></el-input>
                </el-form-item>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="grid-content bg-purple">
            <el-form-item label="城市" prop="city">
              <el-select placeholder="请选则" :v-model="formdata.city">
                <el-option label="区域一" value="111"></el-option>
              </el-select>
            </el-form-item>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="grid-content bg-purple clear">
            <el-form-item>
              <el-button>清除</el-button>
            </el-form-item>
                  <el-form-item>
                    <el-button type="primary">查询</el-button>
                  </el-form-item>
          </div>
        </el-col>
      </el-row>   
     
    </el-form>
    <!-- 状态查询 -->
    <el-row>
      <el-button>全部</el-button>
      <el-button>待审核</el-button>
      <el-button>已审核</el-button>
      <el-button>已拒绝</el-button>
    </el-row>
    <!-- 数据提示 -->
   <el-row>
        <el-alert title="数据一共多少条" type="info" show-icon>
        </el-alert>
   </el-row>
   <!-- 表格 -->
  <el-table :data="tableData" border style="width: 100%">
    <el-table-column fixed prop="number" label="试题编号" width="150">
    </el-table-column>
    <el-table-column prop="subject" label="学科" width="120">
    </el-table-column>
    <el-table-column prop="items" label="目录" width="120">
    </el-table-column>
    <el-table-column prop="questions" label="题型" width="120">
    </el-table-column>
    <el-table-column prop="questionstem" label="题干" width="300">
    </el-table-column>
    <el-table-column prop="inputtime" label="录入时间" width="120">
    </el-table-column>
    <el-table-column prop="difficulty" label="难度" width="120">
    </el-table-column>
    <el-table-column prop="inputpeople" label="录入人" width="120">
    </el-table-column>
    <el-table-column prop="auditstatus" label="审核状态" width="120">
    </el-table-column>
    <el-table-column prop="opinion" label="审核意见" width="120">
    </el-table-column>
    <el-table-column prop="approver" label="审核人" width="120">
    </el-table-column>
    <el-table-column prop="putstatus" label="发布状态" width="120">
    </el-table-column>
    <el-table-column prop="approver" label="审核人" width="120">
    </el-table-column>
    <el-table-column prop="putstatus" label="发布状态" width="120">
    </el-table-column>
      <el-table-column fixed="right" label="操作" width="100" prop="opinion">
        <template slot-scope="scope">
          <el-button @click="handleClick(scope.row)" type="text" size="small">预览</el-button>
          <el-button type="text" size="small">审核</el-button>
          <el-button type="text" size="small">修改</el-button>
          <el-button type="text" size="small">上架</el-button>
          <el-button type="text" size="small">删除</el-button>
        </template>
      </el-table-column>
  </el-table>
   <el-row type="flex" justify="end">
    <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="currentPage4"
      :page-sizes="[100, 200, 300, 400]" :page-size="100" layout="sizes, prev, pager, next, jumper" :total="400">
    </el-pagination>
   </el-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      labelPosition: "right",
      formdata : {
        subject:"",
        seconditem:"",
        tags: "",
        keywords:"",
        type:"",
        difficulty:"",
        direction:"",
        inputpeople: "",
        remark:"",
        companyname:"",
        city:"",

      },
      rules:{},
      
      tableData:[],
      page:{
        currentPage:"",
        pageSize:"",
        total:"",
      },
      currentPage4: 4
    }
  },
  methods: {
    handleClick(row) {
      console.log(row);
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
    }
  },
}
</script>

<style scoped lang='scss'>
.container {
  margin : 10px 10px;
  background-color: #fff;
    .bg-purple {
      color: pink;
      font-size: 10px; 
    }
    .row-bg {
      margin: 0 10px;  
    }
   .subject,.remark,.difficulty {
    margin-left: 25px;
   }
   .clear {
    margin-left: 130px
   }
}
</style>
