<template>
    <div> 
        <button @click="dialogFormVisible=true">打开弹窗</button>
           <!-- 新增弹窗-->
        <el-dialog title="上传文件" :visible.sync="dialogFormVisible" width="40%">
            <el-form :model="form" status-icon :rules="rules" ref="form">
                <el-row type="flex" justify="center">
                    <el-col :span="22">
                        <el-form-item label="上报机构:" label-width="100px" prop="organization">
                            <el-input v-model="form.organization" auto-complete="off"></el-input>
                        </el-form-item>
                    </el-col>
                </el-row>
                <el-row type="flex" justify="center">
                    <el-col :span="22">
                        <el-form-item label="上传文件:" label-width="100px">
                            <el-upload
                                    :limit=limitNum
                                    class="upload-demo"
                                    ref="upload"
                                    :auto-upload="false"
                                    accept=".zip,.rar"
                                    :action="uploadUrl()"
                                    :before-upload="beforeUploadFile"
                                    :on-change="fileChange"
                                    :on-exceed="exceedFile"
                                    :on-success="handleSuccess"
                                    :on-error="handleError"
                                    :with-credentials="true"
                                    :file-list="fileList" 
                                    :on-remove="handleRemove"                                                                   
                            >
                                <el-button size="small" type="primary">点击上传</el-button>
                            </el-upload>
                        </el-form-item>
                    </el-col>
                </el-row>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button size="small" @click="dialogFormVisible = false">取 消</el-button>
                <el-button size="small" type="primary" @click="insert('form')">确 定</el-button>
            </div>
        </el-dialog>
        </div>
</template>
<script>
export default {
    data(){
        return {
            dialogFormVisible:false,
            limitNum: 3,
            form:{
                organization:'',

            },
            fileList:[],
            rules:{
                organization:[
                    { required: true, message: '请输入活动名称', trigger: 'blur' }
                ]
            },
            fileSizeIsSatisfy:true
        }
    },
    methods: {
        exceedFile(files, fileList) {
            this.$message.warning(`只能选择 ${this.limitNum} 个文件`);
        },
       uploadUrl:function(){
        return "http://127.0.0.1:8081/uploadFile";
       },
       // 上传文件之前的钩子, 参数为上传的文件,若返回 false 或者返回 Promise 且被 reject，则停止上传
      beforeUploadFile(file) {
            console.log('before upload',file);
            // let extension = file.name.substring(file.name.lastIndexOf('.')+1);
            // let size = file.size / 1024 / 1024;
            // if(extension !== '.zip' || extension !== '.rar') {
            // this.$message.warning('只能上传后缀是.zip或.rar的文件');
            // }
            // if(size > 5) {
            // this.$message.warning('文件大小不得超过5M');
            // }
        },
    
    fileChange(file,fileList){
        console.log('点击次数')
         //这表示是添加行为
        console.log(`准备修改文件 ${file.raw.name}, 此时文件列表长度为${fileList.length}`);
        if (file.status==='ready') {          
            this.fileList = fileList;
            console.log(`添加了文件 ${file.raw.name}`,file,this.fileList);
            const type = file.raw.type.toLowerCase();
            //console.log(`文件类型为${type}`)
            const isZip = type === 'application/zip' || type === 'application/x-zip-compressed' || type === 'application/octet-stream' || type === 'application/gzip' || type === '';
            if (!isZip) {
                this.fileList.pop();
                this.$message.warning(`上传的文件格式不正确`);
                return;
            }

            // console.log('a',this.fileList)
            // console.log('a',this.fileList.length)
       
            // if(this.fileList.length < 2) {
            //     //this.fileList.unshift(lastFile)
            //     console.log(1,this.fileList)
            // } 
            // else {
            //     console.log(2,Array.from(this.fileList))
                           
            //     var lastFile = this.fileList.pop()   
            //     console.log('b',this.fileList,this.fileList.length)
                
            //     console.log('b',lastFile)
            //     this.fileList.forEach((item,index) => {                   
            //         if(file.raw.name === item.raw.name){
            //            // this.fileList.pop()
            //            //this.fileList = this.fileList.slice(0,this.fileList.length-1)
            //             this.$confirm(`${file.raw.name}文件重复,是否覆盖`, '提示', {
            //                 confirmButtonText: '确定',
            //                 cancelButtonText: '取消',
            //                 type: 'warning'
            //                 }).then(() => {                                                     
            //                 this.fileList[index] = file
            //                 this.fileList.pop()
            //                 this.$message({
            //                     type: 'success',
            //                     message: '覆盖成功!'
            //                 });                          
            //                 }).catch(() => {
            //                 this.$message({
            //                     type: 'info',
            //                     message: '已取消覆盖'
            //                 });          
            //                 });
                        
            //         } else {
            //             this.fileList.push(file)
            //         }
            //     });
            // }
            
            

            // if(this.fileList.length>1){
            //   // const popFile =  this.fileList.pop()
            //    this.fileList = this.fileList.slice(0,this.fileList.length-1)
            //     console.log(1111,this.fileList)
            //     this.fileList && this.fileList.forEach((item,index) => {
            //         if(myFile.name === item.raw.name){
            //             console.log(222,index,this.fileList[index])
            //             console.log(333,file)
            //             this.$confirm(`${myFile.name}文件重复,是否覆盖`, '提示', {
            //                 confirmButtonText: '确定',
            //                 cancelButtonText: '取消',
            //                 type: 'warning'
            //                 }).then(() => {
            //                this.fileList[index] = file
            //                 this.$message({
            //                     type: 'success',
            //                     message: '覆盖成功!'
            //                 });                          
            //                 }).catch(() => {
            //                 this.$message({
            //                     type: 'info',
            //                     message: '已取消覆盖'
            //                 });          
            //                 });
                        
            //         } else {
            //             this.fileList.push(file)
            //         }
            // });




            
            
                
            // });
            // const fileIndex = getFileIndex(file, fileList);
            // if (fileIndex >= 0) {
            //     if (this.replace) {
            //         fileList[fileIndex] = file;
            //         fileList.pop();
            //         this.handleMsg(`${myFile.name}文件重复，已覆盖该文件`);
            //     } else {
            //         fileList.pop();
            //         this.handleMsg(`${myFile.name}文件重复，未覆盖该文件`);
            //     }
            // }
        }
        // //限制上传文件为5M
        var sizeIsSatisfy = file.size < 5*1024*1024 ? true:false;
        if(sizeIsSatisfy){
            return true;
        }else{
            this.fileSizeIsSatisfy = false;
            return false;
        }
    },
    // 文件上传成功时的钩子
      handleSuccess(res, file, fileList) {
        this.$message.success('文件上传成功');
      },
      // 文件上传失败时的钩子
      handleError(err, file, fileList) {
        this.$message.error('文件上传失败');
      },
      insert(formName) {
        console.log(formName)
        this.$refs[formName].validate((valid) => {
            if (valid) {
                if(this.fileList.length <= 0){
                    this.$message.error("请至少上传一个文件！");
                    return;
                }
                if(!this.fileSizeIsSatisfy){
                    this.$message.error("上传失败！存在文件大小超过5M！");
                    return;
                }
                //手动上传文件，在点击确认的时候 校验通过才会去请求上传文件的url
                this.$refs.upload.submit();
                this.postRequest("/api/rcs/sysFile",this.form).then((res)=>{
                    if(res.data.status == 200){
                        this.form = {};
                        this.fileList = [];
                        this.dialogFormVisible = false;
                        this.$message.success('新增成功！');
                    }else{
                        this.$message.error(res.data.msg);
                    }
                })
                    .catch((error)=>{
                        this.$message.error(error)
                    })
            } else {
                this.$message.error('error submit!!');
                return false;
            }
        });
    },
    handleRemove(file, fileList) {
        console.log(2,file, fileList);
      }
    }
}
</script>