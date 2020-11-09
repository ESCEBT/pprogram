<template>
    <div class="fillcontain">
        <head-top></head-top>
        <div class="table_container">
            <el-table
                :data="tableData"
                highlight-current-row
                style="width: 100%">
                <el-table-column
                  type="index"
                  width="100">
                </el-table-column>
                <el-table-column
                  property="uploadTime"
                  label="注册日期"
                  width="220">
                </el-table-column>
                <el-table-column
                  property="username"
                  label="提交人"
                  width="220">
                </el-table-column>
                <el-table-column
                  property="programName"
                  label="项目名称">
                </el-table-column>
            </el-table>
            <div class="Pagination" style="text-align: left;margin-top: 10px;">
                <el-pagination
                  @size-change="handleSizeChange"
                  @current-change="handleCurrentChange"
                  :current-page="currentPage"
                  :page-size="20"
                  layout="total, prev, pager, next"
                  :total="count">
                </el-pagination>
            </div>
        </div>
    </div>
</template>

<script>
    import headTop from '../components/headTop'
    import {getUserList, getUserCount} from '@/api/getData'
    export default {
        data(){
            return {
                tableData: [{
                  uploadTime: '2020-11-02',
                  username: '黄小虎',
                  programName: '项目-团团买菜小程序'
                }, {
                  uploadTime: '2020-11-04',
                  username: '李小红',
                  programName: '学生管理系统的设计与实现'
                }, {
                  uploadTime: '2020-11-05',
                  username: '王小乐',
                  programName: '基于WEB的H5游戏设计开发与实现'
                }, {
                  uploadTime: '2020-11-05',
                  username: '刘大妮',
                  programName: '基于深度学习的图像复原技术'
                }],
                currentRow: null,
                offset: 0,
                limit: 20,
                count: 0,
                currentPage: 1,
            }
        },
    	components: {
    		headTop,
    	},
        created(){
            this.initData();
        },
        methods: {
            async initData(){
                try{
                    const countData = await getUserCount();
                    if (countData.status == 1) {
                        this.count = countData.count;
                    }else{
                      throw new Error('获取数据失败');
                    }
                    this.getUsers();
                }catch(err){
                    this.count = this.tableData.length;
                    console.log('获取数据失败', err);
                }
            },
            handleSizeChange(val) {
                console.log(`每页 ${val} 条`);
            },
            handleCurrentChange(val) {
                this.currentPage = val;
                this.offset = (val - 1)*this.limit;
                this.getUsers()
            },
            async getUsers(){
                const Users = await getUserList({offset: this.offset, limit: this.limit}) || [];
                Users.forEach(item => {
                    tableData.username = item.username;
                    tableData.upload_time = item.upload_time;
                    tableData.city = item.city;
                    this.tableData.push(tableData);
                })
            }
        },
    }
</script>

<style lang="less">
	@import '../style/mixin';
    .table_container{
        padding: 20px;
    }
</style>
