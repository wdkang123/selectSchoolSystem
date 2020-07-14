<template>
  <div id="update_school_box" class="add_school" style="display:none">
    <!-- 操作框 -->
    <div v-on:click="closeAddForm()" style="width:100%;height:60px;background-color:lightblue">
      <div style="width:60px;height:60px;float:right;line-height:60px;font-size:20px;cursor:pointer;">X</div>
    </div>
    <el-form class="data_form" label-width="100px">
      <el-form-item label="学校名称" style="margin-top:20px;width:30%;float:left">
        <el-input  v-model="form.name"></el-input>
      </el-form-item>
      <el-form-item label="中文名称" style="margin-top:20px;width:30%;float:left">
        <el-input v-model="form.name_CN"></el-input>
      </el-form-item>
      <el-form-item label="排名" style="margin-top:20px;width:30%;float:left">
        <el-input v-model="form.rank"></el-input>
      </el-form-item>
      <el-form-item label="tofel" style="margin-top:20px;width:30%;float:left">
        <el-input v-model="form.tofel"></el-input>
      </el-form-item>
      <el-form-item label="ssat" style="margin-top:20px;width:30%;float:left">
        <el-input v-model="form.ssat"></el-input>
      </el-form-item>
      <el-form-item label="类型" style="margin-top:20px;width:30%;float:left">
        <el-input v-model="form.type"></el-input>
      </el-form-item>
      <el-form-item label="site" style="margin-top:20px;width:30%;float:left">
        <el-input v-model="form.site"></el-input>
      </el-form-item>
      <el-form-item label="local" style="margin-top:20px;width:30%;float:left">
        <el-input v-model="form.local"></el-input>
      </el-form-item>
      <el-form-item label="grade_lower" style="margin-top:20px;width:30%;float:left">
        <el-input v-model="form.grade_lower"></el-input>
      </el-form-item>
      <el-form-item label="grade_upper" style="margin-top:20px;width:30%;float:left">
        <el-input v-model="form.grade_upper"></el-input>
      </el-form-item>
      <el-form-item label="宗教" style="margin-top:20px;width:30%;float:left">
        <el-input v-model="form.religion"></el-input>
      </el-form-item>
      <el-form-item label="建校时间" style="margin-top:20px;width:30%;float:left">
        <el-input v-model="form.establish_time"></el-input>
      </el-form-item>
      <el-form-item label="is_boarding" style="margin-top:20px;width:30%;float:left">
        <el-input v-model="form.is_boarding"></el-input>
      </el-form-item>
      <el-form-item label="created_time" style="margin-top:20px;width:30%;float:left">
        <el-input v-model="form.created_time"></el-input>
      </el-form-item>
      <el-form-item label="updated_time" style="margin-top:20px;width:30%;float:left">
        <el-input v-model="form.updated_time"></el-input>
      </el-form-item>
    </el-form>
    <el-button v-on:click="submitFormData()" style="float:left;margin-left:50px;" type="primary">
      提交数据
    </el-button>
  </div>
</template>

<script>
export default {
  name: 'UpdateSchool',
  props:{
    
  },
  data() {
    return {
      form: {}
    }
  },
  created() {
    
  },
  methods: {
    getDataById() {
      this.$get('/school/getSchoolById', {
        'id': this.$parent.rows_id
        }).then(res => {
          this.form = (res.data)[0];
          //window.console.log((res.data[0]));
        });
        //window.console.log("child");
    },
    submitFormData() {
      let add_school_box = document.getElementById("update_school_box");
      add_school_box.style.display = "none";
      this.$get('/school/updSchool', {
        id: this.$parent.rows_id,
        name: this.form.name,
        name_CN: this.form.name_CN,
        rank: this.form.rank,
        tofel: this.form.tofel,
        ssat: this.form.ssat,
        type: this.form.type,
        site: this.form.site,
        local: this.form.local,
        grade_lower: this.form.grade_lower,
        grade_upper: this.form.grade_upper,
        religion: this.form.religion,
        establish_time: this.form.establish_time,
        is_boarding: this.form.is_boarding,
        updated_time: (new Date()).toString(),
      }).then(res => {
        if (res.data == "200") {
          this.submitSuccess();
          this.$parent.getSchoolData();
        } else {
          this.submitError();
        }
      });
      
      //window.console.log(this.$parent.rows_id);
    },
    closeAddForm() {
      let add_school_box = document.getElementById("update_school_box");
      add_school_box.style.display = "none";
    },
    submitSuccess() {
      this.$notify({
        title: '成功',
        message: '提交成功!',
        type: 'success'
      });
    },
    submitError() {
      this.$notify.error({
        title: '错误',
        message: '提交失败'
      });
    }
  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .add_school{
    width: 1000px;
    height:600px;
    position:absolute;
    top:50%;
    bottom:0;
    left:0;
    right:0;
    margin-top:-300px;
    margin-left:auto;
    margin-right:auto;
    /*border: 1px solid black;*/
    background-color: white;
    box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04)
  }
  .data_form{
    width: 1000px;
    height: 400px;
    
  }
  .el-form-item__content{
    width: 30%;

  }
</style>
