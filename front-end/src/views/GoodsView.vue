<template>
    <div id = "home">
        <el-card class="page-container">
            <h1 style="text-align: center">商品管理</h1>
            <div class="float-end">
                <el-button type="success" round @click="exportToExcel">导出 Excel</el-button>
                <el-button type="primary" round @click="showPrintPreview">打印预览</el-button>
            </div>
            <div class="row">
                <div class="col">
                    <el-input placeholder="商品名称" v-model="searchName" @keyup.enter.native="getGoods"></el-input>
                </div>
                <div class="col">
                    <el-input placeholder="类别" v-model="searchCategory" @keyup.enter.native="getGoods"></el-input>
                </div>
                <div class="col">
                    <el-button type="primary" round @click="getGoods">查询</el-button>
                </div>
            </div>
            <el-table :data="curList"
                    border
                    style="width: 100%">
                <el-table-column prop="id" label="id">
                    <template slot-scope="scope">
                        <span>{{ scope.$index + 1 + (page - 1) * size}}</span>
                    </template>
                </el-table-column>
                <el-table-column prop="name" label="名称" width="100px"></el-table-column>
                <el-table-column prop="category" label="分类"></el-table-column>
                <el-table-column prop="origin" label="产地"></el-table-column>
                <el-table-column prop="price" label="价格"></el-table-column>
                <el-table-column prop="productionDate" label="生产日期"></el-table-column>
                <el-table-column prop="manufacturer" label="生产商"></el-table-column>

                <el-table-column label="操作">
                    <template slot-scope="scope">
                        <el-button type="primary" icon="el-icon-edit" round  size="small" @click="getGoodsById(scope.row.id)">编辑</el-button>
                        <el-button type="danger"  icon="el-icon-delete" round  size="small" @click="deleteGoods(scope.row.id)">删除</el-button>
                    </template>
                </el-table-column>
            </el-table>
            <el-dialog title="编辑商品" :visible.sync="dialogUpdateVisible">
                <el-form :model="goods">
                    <el-row>
                        <el-col :span="12">
                            <el-form-item label="id" :label-width="'50px'">
                                <el-input v-model="goods.id" style="width: 240px;" :disabled="true"></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="12">
                            <el-form-item label="名称" :label-width="'50px'">
                                <el-input autocomplete="off" v-model="goods.name" style="width: 240px;"></el-input>
                            </el-form-item>
                        </el-col>
                    </el-row>
                    <el-row>
                        <el-col :span="12">
                            <el-form-item label="分类" :label-width="'50px'">
                                <el-input  autocomplete="off" v-model="goods.category" style="width: 240px;"></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="12">
                            <el-form-item label="产地" :label-width="'50px'">
                                <el-input  autocomplete="off" v-model="goods.origin" style="width: 240px;"></el-input>
                            </el-form-item>
                        </el-col>
                    </el-row>
                    <el-row>
                        <el-col :span="12">
                            <el-form-item label="价格" :label-width="'50px'">
                                <el-input  autocomplete="off" v-model="goods.price" style="width: 240px;"></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="12">
                            <el-form-item label="生产日期">
                                <el-date-picker type="date" placeholder="选择日期" v-model="goods.productionDate" style="width: 240px"></el-date-picker>
                            </el-form-item>
                        </el-col>
                    </el-row>
                    <el-form-item label="生产商">
                        <el-input  autocomplete="off" v-model="goods.manufacturer" style="width: 240px;"></el-input>
                    </el-form-item>
                </el-form>
                <div slot="footer" class="dialog-footer">
                    <el-button @click="dialogUpdateVisible = false">取 消</el-button>
                    <el-button @click="updateGoods" type="primary">确 定</el-button>
                </div>
            </el-dialog>
            <div class = "float-end">
                <el-button type="success" round @click="clearGoods">新增商品</el-button>
            </div>
            <el-dialog title="新增商品" :visible.sync="dialogFormVisible">
                <el-form :model="goods">
                    <el-row>
                        <el-col :span="12">
                            <el-form-item label="名称" :label-width="'50px'">
                                <el-input autocomplete="off" v-model="goods.name" style="width: 240px;"></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="12">
                            <el-form-item label="分类" :label-width="'50px'">
                                <el-input  autocomplete="off" v-model="goods.category" style="width: 240px;"></el-input>
                            </el-form-item>
                        </el-col>
                    </el-row>
                    <el-row>
                        <el-col :span="12">
                            <el-form-item label="产地" :label-width="'50px'">
                                <el-input  autocomplete="off" v-model="goods.origin" style="width: 240px;"></el-input>
                            </el-form-item>
                        </el-col>
                        <el-col :span="12">
                            <el-form-item label="价格" :label-width="'50px'">
                                <el-input  autocomplete="off" v-model="goods.price" style="width: 240px;"></el-input>
                            </el-form-item>
                        </el-col>
                    </el-row>
                    <el-row>
                        <el-col :span="12">
                            <el-form-item label="生产日期">
                                <el-date-picker type="date" placeholder="选择日期" v-model="goods.productionDate" style="width: 240px"></el-date-picker>
                            </el-form-item>
                        </el-col>
                        <el-col :span="12">
                            <el-form-item label="生产商">
                                <el-input  autocomplete="off" v-model="goods.manufacturer" style="width: 240px;"></el-input>
                            </el-form-item>
                        </el-col>
                    </el-row>
                </el-form>
                <div slot="footer" class="dialog-footer">
                    <el-button @click="dialogFormVisible = false">取 消</el-button>
                    <el-button type="primary" @click="addGoods">确 定</el-button>
                </div>
            </el-dialog>
            <el-dialog title="打印预览" :visible.sync="dialogPrintVisible" width="80%">
            <PrintPreview :goodsList="goodsList" />
            </el-dialog>
            <el-pagination background
                        @size-change = "sizeChange"
                        @current-change = "currentChange"
                        :current-page="page"
                        :page-size="size"
                        :page-sizes="pageSizes"
                        layout="total, sizes, prev, pager, next, jumper"
                        :total="total  "
                            style="text-align: center">
            </el-pagination>
            <div class="echarts-box" id="main"></div>
            <!-- <el-carousel :interval="4000" type="card" height="200px">
                <el-carousel-item v-for="item in imageList" :key="item.id">
                    <img :src="item.imgUrl" class="image">
                </el-carousel-item>
            </el-carousel> -->
        </el-card>
    </div>
  </template>
  
  <script>
  
  import axios from '@/utils/axios';
  import * as echarts from "echarts"
  import * as XLSX from 'xlsx'
  import PrintPreview from './PrintPreview.vue';

  export default {
    components: {
      PrintPreview
    },  
    data() {
      return { 
        goods: {
          id: '',
          name: '',
          category: '',
          origin: '',
          price: '',
          productionDate: '',
          manufacturer: '',
        },
        imageList: [
            {id: 0, imgUrl: require("@/assets/preview.jpg")},
            {id: 1, imgUrl: require("@/assets/preview1.jpg")},
            {id: 2, imgUrl: require("@/assets/2024-6-19.jpg")}
        ],
        page: 1,
        size: 5,
        pageSizes: [3, 5, 10, 20, 50, 100, 200, 300, 400, 500, 1000],
        goodsList: [],
        curList: [],
        searchName: '',
        searchCategory: '',
        dialogFormVisible: false,
        dialogUpdateVisible: false,
        dialogPrintVisible: false,
      }
    },
    methods: {
       getGoods() {
           axios.get("/goods", {
            params: {
                searchName: this.searchName,
                searchCategory: this.searchCategory,
            }
        }).then(res => {
            this.goodsList = res.data;
            this.page = 1;
            this.pageQuery()
        }).catch(error => {
            console.error("获取商品失败", error)
        })
      },
      currentChange(val) {
          this.page = val;
          this.pageQuery();
      },
      sizeChange(val) {
          this.size = val;
          this.page = 1;
          this.pageQuery();
      },
      pageQuery() {
        axios.get("/page", {
            params: {
                searchName: this.searchName,
                searchCategory: this.searchCategory,
                page: this.page,
                size: this.size
            }
        }).then(res => {
            this.curList = res.data;
            this.myChart.setOption({
                series: [{
                    data: this.curList.map(item => ({value: item.price, name: item.name}))
                }]
            })
        }).catch(error => {
            console.log(error);
        });
      },
      addGoods() {
          axios.post('/add', this.goods).then(() => {
              this.getGoods();
              this.pageQuery();
              this.dialogFormVisible = false;
              this.$message.success("新增商品成功~");
          }).catch(error => {
              console.log(error);
          })
      },
      deleteGoods(id) {
          this.$confirm('你确定要删除吗?', '提示', {
              confirmButtonText: '确定',
              cancelButtonText: '取消',
              type: 'warning'
          }).then(() => {
              axios.delete(`/delete/${id}`).then(() => {
                  this.getGoods();
                  this.pageQuery();
                  this.$message.success("删除商品成功~");
              }).catch(error => {
                  console.log(error)
              })
          })
      },
      getGoodsById(id) {
          this.dialogUpdateVisible = true;
          axios.get(`/getById/${id}`).then(res => {
              this.goods = res.data;
          }).catch(error => {
              console.log(error);
          })
      },
      updateGoods() {
          axios.put('/update', this.goods).then(() => {
              this.getGoods();
              this.pageQuery();
              this.$message.success("更新商品成功~");
          }).catch(error => {
              console.log(error);
          });
          this.dialogUpdateVisible = false;
      },
      clearGoods() {
          this.goods = {
              id: '',
              name: '',
              category: '',
              origin: '',
              price: '',
              productionDate: '',
              manufacturer: ''
          };
          this.dialogFormVisible = true;
      },
      
      // 导出为 Excel
      exportToExcel() {
          const workSheet = XLSX.utils.json_to_sheet(this.goodsList);
          
          // 手动修改第一行数据
          workSheet["A1"] = { v: "id" };
          workSheet["B1"] = { v: "名称" };
          workSheet["C1"] = { v: "分类" };
          workSheet["D1"] = { v: "产地" };
          workSheet["E1"] = { v: "价格" };
          workSheet["F1"] = { v: "生产日期" };
          workSheet["G1"] = { v: "生产商" };
  
          const colWidths = [{ wpx: 50 }, { wpx: 80 }, { wpx: 80 }, { wpx: 80 }, { wpx: 50 }, { wpx: 100 }, { wpx: 80 }];
          workSheet['!cols'] = colWidths;
    
          const workBook = XLSX.utils.book_new();
          XLSX.utils.book_append_sheet(workBook, workSheet, 'Sheet1');
          XLSX.writeFile(workBook, 'goods.xlsx');
      },
      getEcharts() {
        // 指定图表的配置项和数据
        this.myChart.setOption({
            title: {
                text: '当前页商品展示',
                left: 'center'
            },
            tooltip: {
                trigger: 'item'
            },
            legend: {
                orient: 'vertical',
                left: 'left'
            },
            series: [{
                name: '消费名称',
                type: 'pie',
                radius: '60%',
                data: [],
                emphasis: {
                    itemStyle: {
                        shadowBlur: 10,
                        shadowOffsetX: 0,
                        shadowColor: 'rgba(0,0,0,0.5)'
                    }
                }
            }
            ]
        })
      },
      showPrintPreview() {
        this.dialogPrintVisible = true;
      },
    },
    mounted() {
      this.myChart = echarts.init(document.querySelector('#main'));
      this.getEcharts();
      this.getGoods();
      this.pageQuery();
    },
    computed: {
      total() {
        let tol = this.goodsList.length;
        return tol;
      }
    }
  }
  </script>
  
<style lang="scss" scoped>
.echarts-box {
    width: 500px;
    height: 300px;
    padding-top: 50px;
    margin: 0 auto;
}
.page-container {
  min-height: 100%;
  box-sizing: border-box;

  .header {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
}
.float-end {
    float: right;
}
.row {
  display: flex;
  margin-right: -15px;
  margin-left: -15px;
  margin-bottom: 1rem;
  .col {
    padding-right: 15px;
    padding-left: 15px;
    width: 16%;
  }
}
.el-carousel__item h3 {
    color: #475669;
    font-size: 14px;
    opacity: 0.75;
    line-height: 200px;
    margin: 0;
  }
  
  .el-carousel__item:nth-child(2n) {
    background-color: #99a9bf;
  }
  
  .el-carousel__item:nth-child(2n+1) {
    background-color: #d3dce6;
  }
</style>
  