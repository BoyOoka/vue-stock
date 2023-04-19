<template>
    <el-container style="height: 1080px;">
        <!-- <el-aside width="150px">
        </el-aside> -->
        <el-container>
            <el-header>
                <el-tabs type="border-card" @tab-click="handleClick">
                    <el-tab-pane label="all_code">AllCode</el-tab-pane>
                    <el-tab-pane label="predict">业绩预告</el-tab-pane>
                    <el-tab-pane label="rencent_trade_data">周跌</el-tab-pane>
                    <el-tab-pane label="rencent_up_data">周跌</el-tab-pane>
                </el-tabs>
            </el-header>
            <el-main>
                <el-table
                    :data="tableData"
                    style="width: 100%">
                    <el-table-column
                        prop="code"
                        label="代码"
                        width="100">
                    </el-table-column>
                    <el-table-column
                        prop="name"
                        label="名称"
                        width="100">
                    </el-table-column>
                    <el-table-column
                        prop="wave"
                        label="波动"
                        width="100">
                    </el-table-column>
                    <el-table-column
                        prop="_time"
                        label="创建时间"
                        width="200">
                    </el-table-column>
                    <el-table-column
                        prop="PREDICT_CONTENT"
                        label="内容">
                    </el-table-column>
                </el-table>
            </el-main>
        </el-container>
    </el-container>
</template>
<style>

.el-col {
    border-radius: 4px;
  }
  .bg-purple-dark {
    background: #99a9bf;
  }
  .bg-purple {
    background: #d3dce6;
  }
  .bg-purple-light {
    background: #e5e9f2;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
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
</style>
<script>
  import axios from 'axios'
export default {
    data() {
        return {
            tableData: []
        }
    },
    methods: {
        handleClick(tab, event) {
            console.log(event)
            switch(tab.label){
                case "predict":
                    console.log("predict");
                    // axios.get('http://localhost:5003/home/new_predict')
                    axios.get('/home/new_predict')
                        .then(res => {
                            console.log(res.data);
                            this.tableData = res.data
                        })
                            .catch(err => {
                            console.dir(err)
                        });
                    break
                case "all_code":
                    console.log("all_code");
                    // axios.get('http://localhost:5003/home/all_code')
                    axios.get('/home/all_code')
                    .then(res => {
                        console.log(res.data);
                        this.tableData = res.data
                    })
                        .catch(err => {
                        console.dir(err)
                    });
                    break;
                case "rencent_trade_data":
                    console.log("rencent_trade_data");
                    var data = JSON.stringify({
                        "order": 1,
                        "time_del": 7,
                        "limit": 50
                        });
                    var config = {
                        method: 'post',
                        maxBodyLength: Infinity,
                        // url: 'http://localhost:5003/home/recent_trade_data',
                        url: '/home/recent_trade_data',
                        headers: { 
                            'Content-Type': 'application/json',
                        },
                        data : data
                    };
                    axios.request(config)
                    .then((response) => {
                        this.tableData = response.data
                        console.log(JSON.stringify(response.data));
                    }).catch((error) => {
                        console.log(error);
                        });
                    break;
                case "rencent_up_data":
                    console.log("rencent_up_data");
                    var data_up = JSON.stringify({
                        "order": -1,
                        "time_del": 7,
                        "limit": 50
                        });
                    var config2 = {
                        method: 'post',
                        maxBodyLength: Infinity,
                        // url: 'http://localhost:5003/home/recent_trade_data',
                        url: '/home/recent_trade_data',
                        headers: { 
                            'Content-Type': 'application/json',
                        },
                        data : data_up
                    };
                    axios.request(config2)
                    .then((response) => {
                        this.tableData = response.data
                        console.log(JSON.stringify(response.data));
                    }).catch((error) => {
                        console.log(error);
                        });
                    break;

            }
            
        },
        all_code() {
            // axios.get('http://localhost:5003/home/all_code')
            axios.get('/home/all_code')
            .then(res => {
                console.log(res.data);
                this.message = res.data
            })
                .catch(err => {
                console.dir(err)
            });

        },
        rencent_trade_data(){
            let data = JSON.stringify({
                "order": 1,
                "time_del": 7,
                "limit": 50
                });
            let config = {
                method: 'post',
                maxBodyLength: Infinity,
                // url: 'http://localhost:5003/home/recent_trade_data',
                url: '/home/recent_trade_data',
                headers: { 
                    'Content-Type': 'application/json',
                },
                data : data
                };
                axios.request(config)
                .then((response) => {
                    this.message = response.data
                    console.log(JSON.stringify(response.data));
                })
                .catch((error) => {
                console.log(error);
                });
        }
    }
    }
</script>