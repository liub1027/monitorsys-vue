<template>
    <div class="item">
        <h3 class="list-title">未处理故障信息列表</h3>
        <el-scrollbar class="default-scrollbar" wrap-class="default-scrollbar__wrap" view-class="p20-scrollbar__view"
                      :native="false">
            <div style="overflow-y: auto;height: calc(100% - 40px); width:100%;overflow-x: hidden;">
                <table class="fault-table">
                    <thead>
                        <tr>
                            <th style="width: 50px;"></th>
                            <th>节点</th>
                            <th>故障级别</th>
                            <th>故障位置</th>
                            <th>发生时间</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item, index) in gLog" :key="index" style="cursor: pointer;" @click="showDetail(item)">
                            <td><i class="el-icon-bell" style="color: #ff3003;" @click.stop="goToFaultQuery"></i></td>
                            <td>{{item.ip}}</td>
                            <td><span :class="item.rank==='危险'? orangec:redc">{{item.rank}}</span></td>
                            <td>{{item.location}}</td>
                            <td :title="item.time">{{item.time}}</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </el-scrollbar>
    </div>
</template>

<script>
    export default {
        name: "NotProcessInfo",
        data() {
            return {
                gLog: [],
                orangec:'orange',
                redc:'red'
            }
        },
        mounted(){
            // 使用假数据替代后端API调用
            this.initMockData();
        },
        methods:{
          // 模拟未处理诊断信息数据
          initMockData(){
              this.gLog = [
                  {ip: '192.168.1.101', rank: '危险', time: '2023-06-15 14:30:22', location: '服务器'},
                  {ip: '192.168.1.102', rank: '警告', time: '2023-06-15 14:25:15', location: '路由器'},
                  {ip: '192.168.1.103', rank: '危险', time: '2023-06-15 14:20:45', location: '交换机'},
                  {ip: '192.168.1.104', rank: '警告', time: '2023-06-15 14:15:30', location: '服务器'},
                  {ip: '192.168.1.105', rank: '警告', time: '2023-06-15 14:10:12', location: '路由器'}
              ];
          },
          
          // 跳转到故障查询页面
          goToFaultQuery() {
              // 这里使用Vue Router进行页面跳转
              // 假设故障查询页面的路由路径是'/fault-query'
              this.$router.push('/fault-query');
          },
          
          // 显示故障详情
          showDetail(item) {
              // 使用Element UI的MessageBox组件显示详情
              this.$messagebox({
                  title: '故障详情',
                  message: `
                      <div>
                          <p><strong>节点:</strong> ${item.ip}</p>
                          <p><strong>故障级别:</strong> <span style="color:${item.rank==='危险'?'red':'orange'}">${item.rank}</span></p>
                          <p><strong>故障位置:</strong> ${item.location}</p>
                          <p><strong>发生时间:</strong> ${item.time}</p>
                          <p><strong>故障描述:</strong> ${item.rank === '危险' ? '该设备出现严重故障，需要立即处理' : '该设备出现警告，请尽快检查'}</p>
                      </div>
                  `,
                  dangerouslyUseHTMLString: true,
                  showCancelButton: false,
                  confirmButtonText: '确定'
              });
          }
        }
    }
</script>

<style>
    .item {
        margin-top: 10px;
        position: relative;
        overflow: hidden;
        width: 100%;
        height: 220px;
        border: 1px solid #d0dce2;
        padding: 5px;
        box-sizing: border-box;
        border-radius: 5px;
    }
    
    .list-title {
        text-align: center;
        margin: 0 0 10px 0;
        padding: 5px 0;
        font-size: 16px;
        font-weight: bold;
        color: #333;
    }
    
    .fault-table {
        width: 100%;
        border-collapse: collapse;
    }
    
    .fault-table th {
        background-color: #f5f7fa;
        padding: 8px;
        text-align: left;
        font-weight: bold;
        border-bottom: 2px solid #d0dce2;
    }
    
    .fault-table td {
        padding: 8px;
        border-bottom: 1px solid #d0dce2;
        text-align: left;
    }
    
    .fault-table tr:hover {
        background-color: #f5f7fa;
    }
    
    .fault-table td:first-child {
        text-align: center;
    }
    
    .fault-table i {
        cursor: pointer;
    }
    
    .fault-table i:hover {
        transform: scale(1.1);
        transition: transform 0.2s;
    }
    
    .orange{
        color: orange;
    }
    
    .red{
        color: red;
    }
</style>
