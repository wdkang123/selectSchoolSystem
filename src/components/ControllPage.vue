<template>
    <div>
        <!-- 上边的选择框 -->
        <el-row  class="title_box">
            <el-col :span="6">
                <el-select class="select_box"
                           v-model="value"
                           placeholder="全部院校">
                    <el-option
                            v-for="item in options"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                    </el-option>
                </el-select>
            </el-col>
            <el-col :span="6">
                <el-select  class="select_box"
                           v-model="value"
                           placeholder="全部类型">
                    <el-option
                            v-for="item in options"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                    </el-option>
                </el-select>
            </el-col>
            <el-col :span="6">
                <el-select  class="select_box"
                           v-model="value"
                           placeholder="全部字段">
                    <el-option
                            v-for="item in options"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                    </el-option>
                </el-select>
            </el-col>
            <el-col :span="6">
                <el-button class="search_button">
                    搜索
                </el-button>
            </el-col>
        </el-row>
        <!-- 控制条 -->
        <el-row style="height: 60px;
                       margin-top:-35px;
                       line-height: 60px;">
            <el-col :span="8">
                <el-tabs
                        style="width: 400px"
                        >
                    <el-radio-group v-model="labelPosition"
                                    v-on:change="slideDataBox()"
                                    style="margin-left: -180px;"
                                    size="small">
                        <el-radio-button label="1">
                            信息表
                        </el-radio-button>
                        <el-radio-button label="2">
                            分值表
                        </el-radio-button>
                    </el-radio-group>
                </el-tabs>
            </el-col>
            <el-col :span="8">
                <div class="grid-content bg-purple-light" style="color:white">
                    <el-button v-on:click="addSchoolData()" type="primary" size="mini" style="margin-top:20px;float:right;">
                        新增学校
                    </el-button>
                </div>
            </el-col>
            <el-col :span="8">
                <el-form size="mini" style="margin-top: 20px">
                    <el-form-item>
                        <el-checkbox-group v-model="FormType">
                            <el-checkbox-button label="基本" name="type">

                            </el-checkbox-button>
                            <el-checkbox-button label="氛围" name="type">

                            </el-checkbox-button>
                            <el-checkbox-button label="宗教" name="type">

                            </el-checkbox-button>
                            <el-checkbox-button label="学术" name="type">

                            </el-checkbox-button>
                            <el-checkbox-button label="活动" name="type">

                            </el-checkbox-button>
                            <el-checkbox-button label="生活" name="type">

                            </el-checkbox-button>
                        </el-checkbox-group>
                    </el-form-item>
                </el-form>
            </el-col>
        </el-row>
        <!-- 数据展示框 -->
        <el-row class="controller_box">
            <el-col :span="24">
                <!-- 左右容器结构 -->
                <el-container class="container_box">
                    <!-- 表格信息 -->
                    <!-- school box -->
                    <div>
                        <el-container id="BoxContainer" style="width:100%">
                            <el-table
                                    id="school_data_box"
                                    :data="tableData"
                                    style="width: 100%;"
                                    v-if="show"
                                    :max-height="max_height"
                                    stripe
                                    >   
                                <el-table-column
                                        fixed
                                        prop="name"
                                        label="英文名字"
                                        width="200">
                                </el-table-column>
                                <el-table-column
                                        fixed
                                        prop="name_CN"
                                        label="中文名字"
                                        width="200">
                                </el-table-column>
                                <el-table-column
                                        prop="rank"
                                        label="排名"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        prop="tofel"
                                        label="tofel"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        prop="ssat"
                                        label="ssat"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        prop="type"
                                        label="种类"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        prop="site"
                                        label="site"
                                        width="200">
                                </el-table-column>
                                <el-table-column
                                        prop="local"
                                        label="local"
                                        width="120">
                                </el-table-column>
                                <el-table-column
                                        prop="grade_lower"
                                        label="grade_lower"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        prop="grade_upper"
                                        label="grade_upper"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        prop="religion"
                                        label="religion"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        prop="establish_time"
                                        label="建造时间"
                                        width="120">
                                </el-table-column>
                                <el-table-column
                                        prop="is_boarding"
                                        label="is_boarding"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        prop="created_time"
                                        label="创建时间"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        prop="updated_time"
                                        label="更新时间"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        fixed="right"
                                        label="操作"
                                        width="180">
                                    <template slot-scope="scope">
                                        <el-button
                                                @click.native.prevent="updateDataById(scope.$index, tableData)">
                                            修改
                                        </el-button>
                                        <el-button type="danger" plain
                                                @click.native.prevent="deleteDataById(scope.$index, tableData)">
                                            删除
                                        </el-button>
                                    </template>
                                </el-table-column>
                            </el-table>
<!--
                            <el-table
                                    id="score_data_box"
                                    :data="score_tableData"
                                    style="width: 100%;"
                                    :max-height="max_height"
                                    v-if="!show"
                                    stripe
                                    >   
                                <el-table-column
                                        fixed
                                        prop="name"
                                        label="英文名字"
                                        width="200">
                                </el-table-column>
                                <el-table-column
                                        fixed
                                        prop="name_CN"
                                        label="中文名字"
                                        width="200">
                                </el-table-column>
                                <el-table-column
                                        prop="academic_difficulty"
                                        label="academic_difficulty"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        prop="learning_environment"
                                        label="learning_environment"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        prop="teaching_level"
                                        label="teaching_level"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        prop="t_s_relationship"
                                        label="t_s_relationship"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        prop="physical_environment"
                                        label="physical_environment"
                                        width="200">
                                </el-table-column>
                                <el-table-column
                                        prop="art_environment"
                                        label="art_environment"
                                        width="120">
                                </el-table-column>
                                <el-table-column
                                        prop="club_qualification"
                                        label="club_qualification"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        prop="diet"
                                        label="diet"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        prop="boarding_quality"
                                        label="boarding_quality"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        prop="community_safety"
                                        label="community_safety"
                                        width="120">
                                </el-table-column>
                                <el-table-column
                                        prop="entertainment"
                                        label="entertainment"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        prop="race_equality"
                                        label="race_equality"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        prop="happiness_point"
                                        label="happiness_point"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        prop="pressure_point"
                                        label="pressure_point"
                                        width="100">
                                </el-table-column>
                                <el-table-column
                                        fixed="right"
                                        label="操作"
                                        width="180">
                                    <template slot-scope="scope">
                                        <el-button
                                                @click.native.prevent="updateDataById(scope.$index, tableData)">
                                            修改
                                        </el-button>
                                        <el-button type="danger" plain
                                                @click.native.prevent="deleteDataById(scope.$index, tableData)">
                                            删除
                                        </el-button>
                                    </template>
                                </el-table-column>
                            </el-table>
-->
                        </el-container>
                    </div>
                    <!-- score box -->
                    <div >
                        <el-container>
                            
                        </el-container>
                    </div>
                </el-container>
            </el-col>
        </el-row>

        <!-- 新增框 -->
        <div>
            <AddSchool />
        </div>
        <!-- 修改框 -->
        <div>
            <UpdateSchool ref="UpdateSchool" />
        </div>
    </div>
</template>

<script>
    import AddSchool from './AddSchool'
    import UpdateSchool from './UpdateSchool'

    export default {
        name: "ControllPage",
        components:{
            AddSchool,
            UpdateSchool
        },
        created() {
            this.$get('/school/getAllSchool').then(res => {
                this.tableData = res.data;
            });
        },
        mounted: function () {
            //原生获取屏幕高度
            let orderHight = document.documentElement.clientHeight;
            let orderWidth = document.documentElement.clientWidth;
            document.getElementById('BoxContainer').style.height = orderHight - 180 + 'px';
            document.getElementById("BoxContainer").style.width = orderWidth - 30 + "px";
            this.max_height = orderHight - 180;
            //document.getElementById('score_data_box').style.height = orderHight - 180 + 'px';
        },
        data() {
            return {
                max_height:800,
                labelPosition:'1',
                FormType:[
                    '基本','氛围','宗教','学术','活动','生活'
                ],
                options: [{
                    value: '选项1',
                    label: '黄金糕'
                }, {
                    value: '选项2',
                    label: '双皮奶'
                }, {
                    value: '选项3',
                    label: '蚵仔煎'
                }, {
                    value: '选项4',
                    label: '龙须面'
                }, {
                    value: '选项5',
                    label: '北京烤鸭'
                }],
                value: '',
                //school table data
                tableData: [],
                //school id
                rows_id: 0,
                //score table data
                score_tableData: [],
                //score id
                score_rows_id: 0,
                show:true,
            }
        },
        methods: {
            getSchoolData() {
                this.$get('/school/getAllSchool').then(res => {
                this.tableData = res.data;
            });
            },
            addSchoolData() {
                let add_school_box = document.getElementById("add_school_box");
                add_school_box.style.display = "block";
            },
            handleOpen(key, keyPath) {
                console.log(key, keyPath);
            },
            handleClose(key, keyPath) {
                console.log(key, keyPath);
            },
            updateDataById(index, tables) {
                //rows.splice(index, 1);
                this.rows_id = tables[index]['id'];
                window.console.log(this.rows_id);
                let add_school_box = document.getElementById("update_school_box");
                add_school_box.style.display = "block";
                this.$refs.UpdateSchool.getDataById();
            },
            deleteDataById(index, tables) {
                this.rows_id = tables[index]['id'];
                this.$confirm('此操作将永久删除, 是否继续?', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning'
                    }).then(() => {
                        //delete by id
                        this.$get('/school/delSchoolById', {
                            id:this.rows_id
                        }).then(res => {
                            if (res.data == 200) {
                                this.$message({
                                    type: 'success',
                                    message: '删除成功!'
                                });
                                this.getSchoolData();
                            } else {
                                this.$notify.error({
                                    title: '错误',
                                    message: '提交失败'
                                });
                            }
                        });

                    }).catch(() => {
                        this.$message({
                            type: 'info',
                            message: '已取消删除'
                        });          
                    });
            },
            toSchoolData() {
                this.$message({
                    message: '正在获取最新数据···',
                    type: 'warning'
                });
                this.$get('/school/getAllSchool').then(res => {
                    this.tableData = [];
                    this.tableData = res.data;
                });
                this.show = true;
            },
            toScoreData() {
                this.$message({
                    message: '正在获取最新数据···',
                    type: 'warning'
                });
                this.$get('/score/getAllScoreBySchool').then(res => {
                    this.score_tableData = [];
                    this.score_tableData = res.data;
                });
               this.show = false;
            },
            slideDataBox() {
                //window.console.log("----");
                //window.console.log(this.labelPosition);
                //window.console.log("----");
                if (this.labelPosition == '1') {
                    this.toSchoolData();
                } else {
                    this.toScoreData();
                }
            }
        }
    }
</script>

<style scoped>
    .el-header, .el-footer {
        background-color: #B3C0D1;
        color: #333;
        text-align: center;
        line-height: 60px;
    }

    .el-aside {
        background-color: #D3DCE6;
        color: #333;
        text-align: center;
        line-height: 200px;
    }

    .el-main {
        background-color: #E9EEF3;
        color: #333;
        text-align: center;
        line-height: 160px;
    }

    body > .el-container {
        margin-bottom: 40px;
    }

    .el-container:nth-child(5) .el-aside,
    .el-container:nth-child(6) .el-aside {
        line-height: 260px;
    }

    .el-container:nth-child(7) .el-aside {
        line-height: 320px;
    }



    .title_box{
        /*background: red;*/
        height: 100px;
    }
    .controller_box{
        height: 650px;
    }
    .container_box{
        width: 98%;
        height: 600px;
        top:0;
        bottom:0;
        left:0;
        right:0;
        margin-left: auto;
        margin-right: auto;
    }
    .select_box{
        width: 80%;
        height: 40px;
        margin-top: 30px;
    }
    .search_button{
        width: 124px;
        height: 40px;
        margin-top: 30px;
        background: dodgerblue;
        color:white;
        line-height: 16px;
        font-size:16px;
        font-weight:bold;
        box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1)
    }
</style>
