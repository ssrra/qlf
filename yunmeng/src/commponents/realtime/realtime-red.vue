<template>
  <div class="realtime-red">
     <div class="nav">
      <h1>红外相机-实时数据</h1>
      <i class="el-icon-s-unfold" @click="goback()"></i>
      <a>|</a>
      <span>管理员</span>
      <div class="circle"><i class="el-icon-s-custom" ></i></div>
    </div>

    <div class="box">
      <div class="left">
        <!-- 地图 -->
        <div id="container"></div>
        <div class="smallbox" v-show="showGroup"></div>
      </div>

      <div class="right">
        <!-- 实时监测 -->
        <div class="top">
          <h3>实时监测</h3>
            <div class="list1">
              <el-table
                :data="tableData1"
                height="241"
                border
                style="width: 100%">
                <el-table-column
                  prop="camera"
                  label="监测点位"
                  width="105">
                </el-table-column>
                <el-table-column
                  prop="name"
                  label="视频"
                  width="240">
                </el-table-column>
                <el-table-column
                  prop="date"
                  label="时间"
                  width="240">
                </el-table-column>
              </el-table>
            </div>
        </div>
        <!-- 当前布设相机 -->
        <div class="bottom">
          <h3>当前布设相机</h3>
          <div class="list2">
            <el-table
              :data="tableData2"
              height="391"
              border
              style="width: 100%">
              <el-table-column
                prop="name"
                label="名称"
                width="130">
              </el-table-column>
              <el-table-column
                prop="id"
                label="监测时间"
                width="190">
              </el-table-column>
              <el-table-column
                prop="num"
                label="今日数量"
                width="110">
              </el-table-column>
               <el-table-column
                  fixed="right"
                  label="操作"
                  width="150">
                   <template slot-scope="scope">
                    <el-button @click="handleClick(scope.row)" type="text" size="small">编辑</el-button>
                    <el-button
                      @click.native.prevent="deleteRow(scope.$index, tableData)"
                      type="text"
                      size="small">
                      移除
                    </el-button>
                  </template>
                </el-table-column>
            </el-table>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>
 
<script>

export default {
  data(){
        return{
            showGroup: false,
            longitude: 36.26153,
            latitude: 106.39212,
            tableData1: [{
              camera: '001',
              name: '001-2020-09-25.mp4',
              date: '2020-09-25'
            }, {
              camera: '002',
              name: '001-2020-09-25.mp4',
              date: '2020-09-25'
            }, {
              camera: '003',
              name: '001-2020-09-25.mp4',
              date: '2020-09-25'
            }, {
              camera: '004',
              name: '001-2020-09-25.mp4',
              date: '2020-09-25'
            }],
            tableData2: [{
              name: '松鼠',
              id: '001-2020-09-25.mp4',
              num: '3'
            }, {
              name: '松鼠',
              id: '001-2020-09-25.mp4',
              num: '3'
            }, {
              name: '松鼠',
              id: '001-2020-09-25.mp4',
              num: '3'
            }, {
              name: '松鼠',
              id: '001-2020-09-25.mp4',
              num: '3'
            }, {
              name: '松鼠',
              id: '001-2020-09-25.mp4',
              num: '3'
            }, {
              name: '松鼠',
              id: '001-2020-09-25.mp4',
              num: '3'
            }]
        }
  },
  methods:{
    goback(){
      this.$router.push('/home')
    },
    // 腾讯地图
    setMap() {
        //步骤：定义map变量 调用 qq.maps.Map() 构造函数   获取地图显示容器
        //设置地图中心点
        var myLatlng = new qq.maps.LatLng(this.longitude,this.latitude);
        //定义工厂模式函数
        var myOptions = {
            zoom: 12,               //设置地图缩放级别
            zoomControl: false,
            panControl: false,
            mapTypeControl: false,
            center: myLatlng,      
            mapTypeId: qq.maps.MapTypeId.SATELLITE  
        }
        //获取dom元素添加地图信息
        var map = new qq.maps.Map(document.getElementById("container"), myOptions);
        var marker = new qq.maps.Marker({ 
            position: myLatlng,
            animation: qq.maps.MarkerAnimation.DOWN,
            map:map,
            draggable: false,
            icon: new qq.maps.MarkerImage('https://3gimg.qq.com/lightmap/api_v2/2/4/99/theme/default/imgs/marker.png'),
        });
        //添加到提示窗
        var info = new qq.maps.InfoWindow({
            map: map
        });
        //获取标记的点击事件
        qq.maps.event.addListener(marker, 'click', function() {
          // this.showGroup = !show
            info.open(); 
            info.setContent(
              '<div style="text-align:center;white-space:nowrap;'+'margin:20px;">单击标记</div>'
            );
            info.setPosition(myLatlng); 
        });
        marker.setVisible(true);
    }
  },
  mounted(){
    this.setMap();
  }
}
</script>

<style scoped>
  .box{
    width: 98%;
    height: 53.5rem;
    background-color:white;
    margin: 0 auto;
    margin-top: 1rem;
    /* position: fixed; */
  }
  .nav{
    width: 100%;
    height: 3rem;
    background-color: white;
  }
  .realtime-red h1{
    width: 50%;
    float: left;
    font-size: 1.14rem;
    line-height: 3rem;
    margin-left: 1.2rem;
  }
  .el-icon-s-unfold{
    float: right;
    margin-right: 15px;
    font-size: 3rem;
    cursor: pointer;
    color: gray;
  }
  .el-icon-s-unfold:hover{
    color: black;
  }
.el-icon-s-custom{
  float: right;
  font-size: 2rem;
  cursor: pointer;
  color: white;
  margin-right: 3px;
  margin-top: 1px;
}
.nav span{
  float: right;
  line-height: 3rem;
  cursor: pointer;
  margin-right: 20px;
}
.nav a{
  float: right;
  line-height: 3rem;
  margin-right: 20px;
}
.circle{
  float: right;
  width: 40px;
  height: 40px;
  border: 1px solid #4682B4;
  border-radius: 100px;
  margin-top: 4px;
  background-color: #4682B4;
  margin-right: 10px;
}


    /* left */
  .left{
    width: 65%;
    height: 100%;
    float: left;
  }
  #container{
    border: 2px solid gray;
    width: 97%;
    height: 94%;
    margin: 2.1% auto;
    border-radius: 5px;
  }

  /* right */
  .right{
    width: 35%;
    height: 100%;
    float: left;
    padding: 1.4% 0;
  }
  /* 实时监测 */
  .top{
    width: 100%;
    height: 40%;
  }
  .top h3{
    width: 100%;
    padding: 10px 0 0 10px;
  }
  /* 当前布设相机 */
  .bottom{
    width: 100%;
    height: 60%;
  }
  .bottom h3{
    width: 100%;
    padding: 10px 0 0 10px;
  }

/* 实时监测 */
.list1{
  width: 100%;
  height: 20%;
  margin-top: 20px;
}

/* 当前布设相机 */
.list2{
  width: 100%;
  height: 93%;
  margin-top: 20px;
}




.smallbox{
  width: 150px;
  height: 250px;
  background-color: gray;
  border-radius: 5px;
}
</style>