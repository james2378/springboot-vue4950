<template>
<div :style='{"padding":"20px","boxShadow":"0px 4px 10px 0px rgba(0,0,0,0.302)","margin":"20px auto 30px","borderRadius":"20px","background":"#fff","width":"1100px","position":"relative"}'>
    <el-form
      class="add-update-preview"
      ref="ruleForm"
      :model="ruleForm"
      :rules="rules"
      label-width="120px"
    >
          <el-form-item :style='{"padding":"10px","margin":"0 0 10px"}' label="物品编号" prop="wupinbianhao">
              <el-input v-model="ruleForm.wupinbianhao" placeholder="物品编号" readonly></el-input>
          </el-form-item>
          <el-form-item :style='{"padding":"10px","margin":"0 0 10px"}' label="物品名称" prop="wupinmingcheng">
            <el-input v-model="ruleForm.wupinmingcheng" 
                placeholder="物品名称" clearable ></el-input>
          </el-form-item>
          <el-form-item :style='{"padding":"10px","margin":"0 0 10px"}' label="品牌" prop="pinpai">
            <el-input v-model="ruleForm.pinpai" 
                placeholder="品牌" clearable ></el-input>
          </el-form-item>
          <el-form-item :style='{"padding":"10px","margin":"0 0 10px"}'  label="物品类别" prop="wupinleibie">
            <el-select v-model="ruleForm.wupinleibie" placeholder="请选择物品类别"  >
              <el-option
                  v-for="(item,index) in wupinleibieOptions"
                  :key="index"
                  :label="item"
                  :value="item">
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item :style='{"padding":"10px","margin":"0 0 10px"}' label="物品图片" v-if="type!='cross' || (type=='cross' && !ro.wupintupian)" prop="wupintupian">
            <file-upload
            tip="点击上传物品图片"
            action="file/upload"
            :limit="3"
            :multiple="true"
            :fileUrls="ruleForm.wupintupian?ruleForm.wupintupian:''"
            @change="wupintupianUploadChange"
            ></file-upload>
          </el-form-item>
            <el-form-item :style='{"padding":"10px","margin":"0 0 10px"}' class="upload" v-else label="物品图片" prop="wupintupian">
                <img v-if="ruleForm.wupintupian.substring(0,4)=='http'" class="upload-img" style="margin-right:20px;" v-bind:key="index" :src="ruleForm.wupintupian.split(',')[0]" width="100" height="100">
                <img v-else class="upload-img" style="margin-right:20px;" v-bind:key="index" v-for="(item,index) in ruleForm.wupintupian.split(',')" :src="baseUrl+item" width="100" height="100">
            </el-form-item>
          <el-form-item :style='{"padding":"10px","margin":"0 0 10px"}' label="新旧程度" prop="xinjiuchengdu">
            <el-input v-model="ruleForm.xinjiuchengdu" 
                placeholder="新旧程度" clearable ></el-input>
          </el-form-item>
          <el-form-item :style='{"padding":"10px","margin":"0 0 10px"}' label="出租数量" prop="wupinshuliang">
            <el-input v-model="ruleForm.wupinshuliang" 
                placeholder="出租数量" clearable ></el-input>
          </el-form-item>
          <el-form-item :style='{"padding":"10px","margin":"0 0 10px"}' label="租金" prop="zujin">
            <el-input v-model="ruleForm.zujin" 
                placeholder="租金" clearable ></el-input>
          </el-form-item>
          <el-form-item :style='{"padding":"10px","margin":"0 0 10px"}' label="押金" prop="yajin">
            <el-input v-model="ruleForm.yajin" 
                placeholder="押金" clearable ></el-input>
          </el-form-item>
          <el-form-item :style='{"padding":"10px","margin":"0 0 10px"}' label="发布时间" prop="fabushijian" >
              <el-date-picker
                  format="yyyy 年 MM 月 dd 日"
                  value-format="yyyy-MM-dd"
                  v-model="ruleForm.fabushijian" 
                  type="date"
                  placeholder="发布时间">
              </el-date-picker> 
          </el-form-item>
          <el-form-item :style='{"padding":"10px","margin":"0 0 10px"}' label="出租账号" prop="yonghuzhanghao">
            <el-input v-model="ruleForm.yonghuzhanghao" 
                placeholder="出租账号" clearable ></el-input>
          </el-form-item>
          <el-form-item :style='{"padding":"10px","margin":"0 0 10px"}' label="出租姓名" prop="yonghuxingming">
            <el-input v-model="ruleForm.yonghuxingming" 
                placeholder="出租姓名" clearable ></el-input>
          </el-form-item>
          <el-form-item :style='{"padding":"10px","margin":"0 0 10px"}' label="出租电话" prop="yonghudianhua">
            <el-input v-model="ruleForm.yonghudianhua" 
                placeholder="出租电话" clearable ></el-input>
          </el-form-item>
          <el-form-item :style='{"padding":"10px","margin":"0 0 10px"}' label="物品详情" prop="wupinxiangqing">
            <editor 
                :style='{"padding":"0","boxShadow":"0px 4px 10px 0px rgba(0,0,0,0.302)","margin":"0","borderColor":"#ccc","backgroundColor":"#fff","borderRadius":"0","borderWidth":"0","width":"100%","borderStyle":"solid","height":"auto"}'
                v-model="ruleForm.wupinxiangqing" 
                class="editor" 
                action="file/upload">
            </editor>
          </el-form-item>

      <el-form-item :style='{"padding":"0","margin":"0"}'>
        <el-button :style='{"border":"0","cursor":"pointer","padding":"0","margin":"0 20px 0 0","outline":"none","color":"rgba(255, 255, 255, 1)","borderRadius":"4px","background":"rgba(67, 144, 165, 1)","width":"128px","lineHeight":"40px","fontSize":"14px","height":"40px"}'  type="primary" @click="onSubmit">提交</el-button>
        <el-button :style='{"border":"1px solid rgba(67, 144, 165, 1)","cursor":"pointer","padding":"0","margin":"0","outline":"none","color":"rgba(67, 144, 165, 1)","borderRadius":"4px","background":"rgba(255, 255, 255, 1)","width":"128px","lineHeight":"40px","fontSize":"14px","height":"40px"}' @click="back()">返回</el-button>
      </el-form-item>
    </el-form>
</div>
</template>

<script>
  export default {
    data() {
      return {
        id: '',
        baseUrl: '',
        ro:{
            wupinbianhao : false,
            wupinmingcheng : false,
            pinpai : false,
            wupinleibie : false,
            wupintupian : false,
            xinjiuchengdu : false,
            wupinshuliang : false,
            wupinxiangqing : false,
            zujin : false,
            yajin : false,
            fabushijian : false,
            yonghuzhanghao : false,
            yonghuxingming : false,
            yonghudianhua : false,
            clicktime : false,
            clicknum : false,
        },
        type: '',
        userTableName: localStorage.getItem('UserTableName'),
        ruleForm: {
          wupinbianhao: this.getUUID(),
          wupinmingcheng: '',
          pinpai: '',
          wupinleibie: '',
          wupintupian: '',
          xinjiuchengdu: '',
          wupinshuliang: '',
          wupinxiangqing: '',
          zujin: '',
          yajin: '',
          fabushijian: '',
          yonghuzhanghao: '',
          yonghuxingming: '',
          yonghudianhua: '',
          clicktime: '',
          clicknum: '',
        },
        wupinleibieOptions: [],


        rules: {
          wupinbianhao: [
          ],
          wupinmingcheng: [
            { required: true, message: '物品名称不能为空', trigger: 'blur' },
          ],
          pinpai: [
          ],
          wupinleibie: [
          ],
          wupintupian: [
          ],
          xinjiuchengdu: [
            { required: true, message: '新旧程度不能为空', trigger: 'blur' },
          ],
          wupinshuliang: [
            { validator: this.$validate.isIntNumer, trigger: 'blur' },
          ],
          wupinxiangqing: [
          ],
          zujin: [
          ],
          yajin: [
          ],
          fabushijian: [
          ],
          yonghuzhanghao: [
          ],
          yonghuxingming: [
          ],
          yonghudianhua: [
          ],
          clicktime: [
          ],
          clicknum: [
            { validator: this.$validate.isIntNumer, trigger: 'blur' },
          ],
        },
      };
    },
    computed: {



    },
    components: {
    },
    created() {
	  //this.bg();
      let type = this.$route.query.type ? this.$route.query.type : '';
      this.init(type);
      this.baseUrl = this.$config.baseUrl;
      this.ruleForm.fabushijian = this.getCurDate()
    },
    methods: {
      getMakeZero(s) {
          return s < 10 ? '0' + s : s;
      },
      // 下载
      download(file){
        window.open(`${file}`)
      },
      // 初始化
      init(type) {
        this.type = type;
        if(type=='cross'){
          var obj = JSON.parse(localStorage.getItem('crossObj'));
          for (var o in obj){
            if(o=='wupinbianhao'){
              this.ruleForm.wupinbianhao = obj[o];
              this.ro.wupinbianhao = true;
              continue;
            }
            if(o=='wupinmingcheng'){
              this.ruleForm.wupinmingcheng = obj[o];
              this.ro.wupinmingcheng = true;
              continue;
            }
            if(o=='pinpai'){
              this.ruleForm.pinpai = obj[o];
              this.ro.pinpai = true;
              continue;
            }
            if(o=='wupinleibie'){
              this.ruleForm.wupinleibie = obj[o];
              this.ro.wupinleibie = true;
              continue;
            }
            if(o=='wupintupian'){
              this.ruleForm.wupintupian = obj[o].split(",")[0];
              this.ro.wupintupian = true;
              continue;
            }
            if(o=='xinjiuchengdu'){
              this.ruleForm.xinjiuchengdu = obj[o];
              this.ro.xinjiuchengdu = true;
              continue;
            }
            if(o=='wupinshuliang'){
              this.ruleForm.wupinshuliang = obj[o];
              this.ro.wupinshuliang = true;
              continue;
            }
            if(o=='wupinxiangqing'){
              this.ruleForm.wupinxiangqing = obj[o];
              this.ro.wupinxiangqing = true;
              continue;
            }
            if(o=='zujin'){
              this.ruleForm.zujin = obj[o];
              this.ro.zujin = true;
              continue;
            }
            if(o=='yajin'){
              this.ruleForm.yajin = obj[o];
              this.ro.yajin = true;
              continue;
            }
            if(o=='fabushijian'){
              this.ruleForm.fabushijian = obj[o];
              this.ro.fabushijian = true;
              continue;
            }
            if(o=='yonghuzhanghao'){
              this.ruleForm.yonghuzhanghao = obj[o];
              this.ro.yonghuzhanghao = true;
              continue;
            }
            if(o=='yonghuxingming'){
              this.ruleForm.yonghuxingming = obj[o];
              this.ro.yonghuxingming = true;
              continue;
            }
            if(o=='yonghudianhua'){
              this.ruleForm.yonghudianhua = obj[o];
              this.ro.yonghudianhua = true;
              continue;
            }
            if(o=='clicktime'){
              this.ruleForm.clicktime = obj[o];
              this.ro.clicktime = true;
              continue;
            }
            if(o=='clicknum'){
              this.ruleForm.clicknum = obj[o];
              this.ro.clicknum = true;
              continue;
            }
          }
        }
        // 获取用户信息
        this.$http.get(this.userTableName + '/session', {emulateJSON: true}).then(res => {
          if (res.data.code == 0) {
            var json = res.data.data;
            if((json.yonghuzhanghao!=''&&json.yonghuzhanghao) || json.yonghuzhanghao==0){
                this.ruleForm.yonghuzhanghao = json.yonghuzhanghao
            }
            if((json.yonghuxingming!=''&&json.yonghuxingming) || json.yonghuxingming==0){
                this.ruleForm.yonghuxingming = json.yonghuxingming
            }
            if((json.yonghudianhua!=''&&json.yonghudianhua) || json.yonghudianhua==0){
                this.ruleForm.yonghudianhua = json.yonghudianhua
            }
          }
        });
        this.$http.get('option/wupinleibie/wupinleibie', {emulateJSON: true}).then(res => {
          if (res.data.code == 0) {
            this.wupinleibieOptions = res.data.data;
          }
        });
      },

    // 多级联动参数
      // 多级联动参数
      info(id) {
        this.$http.get('wupinxinxi/detail/${id}', {emulateJSON: true}).then(res => {
          if (res.data.code == 0) {
            this.ruleForm = res.data.data;
          }
        });
      },
      // 提交
      onSubmit() {

        //更新跨表属性
        var crossuserid;
        var crossrefid;
        var crossoptnum;
        this.$refs["ruleForm"].validate(valid => {
          if(valid) {
            if(this.type=='cross'){
                 var statusColumnName = localStorage.getItem('statusColumnName');
                 var statusColumnValue = localStorage.getItem('statusColumnValue');
                 if(statusColumnName && statusColumnName!='') {
                     var obj = JSON.parse(localStorage.getItem('crossObj'));
                     if(!statusColumnName.startsWith("[")) {
                         for (var o in obj){
                             if(o==statusColumnName){
                                 obj[o] = statusColumnValue;
                             }
                         }
                         var table = localStorage.getItem('crossTable');
                         this.$http.post(table+'/update', obj).then(res => {});
                     } else {
                            crossuserid=Number(localStorage.getItem('userid'));
                            crossrefid=obj['id'];
                            crossoptnum=localStorage.getItem('statusColumnName');
                            crossoptnum=crossoptnum.replace(/\[/,"").replace(/\]/,"");
                     }
                 }
            }
            if(crossrefid && crossuserid) {
                 this.ruleForm.crossuserid=crossuserid;
                 this.ruleForm.crossrefid=crossrefid;
                 var params = {
                     page: 1,
                     limit: 10,
                     crossuserid:crossuserid,
                     crossrefid:crossrefid,
                 }
                 this.$http.get('wupinxinxi/list', {
                  params: params
                 }).then(res => {
                     if(res.data.data.total>=crossoptnum) {
                         this.$message({
                          message: localStorage.getItem('tips'),
                          type: 'success',
                          duration: 1500,
                         });
                          return false;
                     } else {
                         // 跨表计算


                          this.$http.post('wupinxinxi/add', this.ruleForm).then(res => {
                              if (res.data.code == 0) {
                                  this.$message({
                                      message: '操作成功',
                                      type: 'success',
                                      duration: 1500,
                                      onClose: () => {
                                          this.$router.go(-1);
                                      }
                                  });
                              } else {
                                  this.$message({
                                      message: res.data.msg,
                                      type: 'error',
                                      duration: 1500
                                  });
                              }
                          });
                     }
                 });
             } else {


                  this.$http.post('wupinxinxi/add', this.ruleForm).then(res => {
                     if (res.data.code == 0) {
                          this.$message({
                              message: '操作成功',
                              type: 'success',
                              duration: 1500,
                              onClose: () => {
                                  this.$router.go(-1);
                              }
                          });
                      } else {
                          this.$message({
                              message: res.data.msg,
                              type: 'error',
                              duration: 1500
                          });
                      }
                  });
             }
          }
        });
      },
      // 获取uuid
      getUUID () {
        return new Date().getTime();
      },
      // 返回
      back() {
        this.$router.go(-1);
      },
      wupintupianUploadChange(fileUrls) {
          this.ruleForm.wupintupian = fileUrls.replace(new RegExp(this.$config.baseUrl,"g"),"");;
      },
    }
  };
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
	.el-date-editor.el-input {
		width: auto;
	}
	
	.add-update-preview .el-form-item /deep/ .el-form-item__label {
	  padding: 0 10px 0 0;
	  color: #666;
	  font-weight: 500;
	  width: 120px;
	  font-size: 14px;
	  line-height: 40px;
	  text-align: right;
	}
	
	.add-update-preview .el-form-item /deep/ .el-form-item__content {
	  margin-left: 120px;
	}
	
	.add-update-preview .el-input /deep/ .el-input__inner {
	  border: 0;
	  border-radius: 4px;
	  padding: 0 12px;
	  box-shadow: 0px 4px 10px 0px rgba(0,0,0,0.302);
	  outline: none;
	  color: #333;
	  width: 300px;
	  font-size: 14px;
	  height: 40px;
	}
	
	.add-update-preview .el-select /deep/ .el-input__inner {
	  border: 0;
	  border-radius: 4px;
	  padding: 0 10px;
	  box-shadow: 0px 4px 10px 0px rgba(0,0,0,0.302);
	  outline: none;
	  color: #333;
	  width: 200px;
	  font-size: 14px;
	  height: 40px;
	}
	
	.add-update-preview .el-date-editor /deep/ .el-input__inner {
	  border: 0;
	  border-radius: 4px;
	  padding: 0 10px 0 30px;
	  box-shadow: 0px 4px 10px 0px rgba(0,0,0,0.302);
	  outline: none;
	  color: #333;
	  width: 200px;
	  font-size: 14px;
	  height: 40px;
	}
	
	.add-update-preview /deep/ .el-upload--picture-card {
		background: transparent;
		border: 0;
		border-radius: 0;
		width: auto;
		height: auto;
		line-height: initial;
		vertical-align: middle;
	}
	
	.add-update-preview /deep/ .upload .upload-img {
	  border: 1px dashed #666;
	  cursor: pointer;
	  border-radius: 6px;
	  color: #666;
	  width: 100px;
	  font-size: 32px;
	  line-height: 100px;
	  text-align: center;
	  height: 100px;
	}
	
	.add-update-preview /deep/ .el-upload-list .el-upload-list__item {
	  border: 1px dashed #666;
	  cursor: pointer;
	  border-radius: 6px;
	  color: #666;
	  width: 100px;
	  font-size: 32px;
	  line-height: 100px;
	  text-align: center;
	  height: 100px;
	}
	
	.add-update-preview /deep/ .el-upload .el-icon-plus {
	  border: 1px dashed #666;
	  cursor: pointer;
	  border-radius: 6px;
	  color: #666;
	  width: 100px;
	  font-size: 32px;
	  line-height: 100px;
	  text-align: center;
	  height: 100px;
	}
	
	.add-update-preview .el-textarea /deep/ .el-textarea__inner {
	  border: 0;
	  border-radius: 4px;
	  padding: 12px;
	  box-shadow: 0px 4px 10px 0px rgba(0,0,0,0.302);
	  outline: none;
	  color: #333;
	  width: 400px;
	  font-size: 14px;
	  height: 120px;
	}
</style>
