<template>
    <div >

        <el-row :gutter="20">
            <!-- 左侧搜索栏，占地面积 20/24 -->
            <el-col :span="20">
                <el-form
                    :inline="true"
                    :model="containerQueryContent"
                    class="el-form--inline"
                >
                    <el-form-item :label="'容器类型'">
                        <el-select v-model="containerQueryContent.sourceType" :placeholder="'容器类型'">
                            <el-option
                                    v-for="item in sourceTypeOptions"
                                    :key="item.key"
                                    :label="item.label"
                                    :value="item.key">
                            </el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item :label="$t('message.keyword')">
                        <el-input
                            v-model="containerQueryContent.keyword"
                            :placeholder="$t('message.keyword')"
                        />
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" @click="listContainer">{{
                            $t('message.query')
                        }}</el-button>
                        <el-button type="cancel" @click="onClickReset">{{
                            $t('message.reset')
                        }}</el-button>
                    </el-form-item>
                </el-form>
            </el-col>

            <!-- 右侧新增任务按钮，占地面积 4/24 -->
            <el-col :span="4">
                <div style="float:right;padding-right:10px">
                    <el-button
                    style="float: right; "
                    type="primary"
                    @click="onClickNewContainer"
                    >{{ $t('message.newContainer') }}</el-button
                >
                </div>
            </el-col>
        </el-row>

        <!--第二行，任务数据表格-->
        <el-row>
            <el-table :data="containerPageResult.data" style="width: 100%">
                <el-table-column :label="$t('message.containerId')" prop="id">
                </el-table-column>
                <el-table-column
                    :label="$t('message.containerName')"
                    prop="containerName"
                >
                </el-table-column>
                <el-table-column
                    :label="$t('message.containerType')"
                    prop="sourceType"
                >
                </el-table-column>
                <el-table-column
                    :label="'容器描述'"
                    prop="containerDesc"
                >
                </el-table-column>
                <el-table-column
                    :label="$t('message.containerVersion')"
                    prop="version"
                >
                </el-table-column>
                <el-table-column v-if="false"
                    :label="'压缩包中文件路径'"
                    prop="containerExecPath"
                >
                </el-table-column>
                <el-table-column
                    :label="$t('message.deployTime')"
                    prop="lastDeployTime"
                >
                </el-table-column>
                <el-table-column :label="$t('message.status')" prop="status">
                </el-table-column>
                <el-table-column label="操作" width="400">
                    <template slot-scope="scope">
                        <el-button
                            type="primary"
                            @click="arrangeItem(scope.row)"
                            >{{ $t('message.deploy') }}</el-button
                        >
                        <el-button
                            type="primary"
                            @click="editItem(scope.row)"
                            >{{ $t('message.edit') }}</el-button
                        >
                        <el-button
                            type="primary"
                            @click="deleteItem(scope.row, scope.$index)"
                            >{{ $t('message.delete') }}</el-button
                        >
                        <el-button
                            type="primary"
                            @click="listOfItem(scope.row)"
                            >{{ $t('message.deployedWorkerList') }}</el-button
                        >
                    </template>
                </el-table-column>
            </el-table>
        </el-row>

        <!-- 第三行，分页插件 -->
        <el-row>
            <el-pagination
                layout="prev, pager, next"
                :total="this.containerPageResult.totalItems"
                :page-size="this.containerPageResult.pageSize"
                :current-page.sync="current"
                @current-change="onClickChangePage"
                :hide-on-single-page="true"
            />
        </el-row>
        <!-- <el-card class="box-card">
            <div slot="header" class="clearfix">
                <span></span>
                <el-button  style="float: left; " type="primary" @click="listContainer"
                    >{{$t('message.refresh')}}</el-button
                >
                <el-button
                    style="float: right; "
                    type="primary"
                    @click="onClickNewContainer"
                    >{{ $t('message.newContainer') }}</el-button
                >
            </div>
            <el-table :data="containerList" style="width: 100%">
                <el-table-column :label="$t('message.containerId')" prop="id">
                </el-table-column>
                <el-table-column
                    :label="$t('message.containerName')"
                    prop="containerName"
                >
                </el-table-column>
                <el-table-column
                    :label="$t('message.containerType')"
                    prop="sourceType"
                >
                </el-table-column>
                <el-table-column
                    :label="$t('message.containerVersion')"
                    prop="version"
                >
                </el-table-column>
                <el-table-column
                    :label="$t('message.deployTime')"
                    prop="lastDeployTime"
                >
                </el-table-column>
                <el-table-column :label="$t('message.status')" prop="status">
                </el-table-column>
                <el-table-column label="操作" width="400">
                    <template slot-scope="scope">
                        <el-button
                            type="primary"
                            @click="arrangeItem(scope.row)"
                            >{{ $t('message.deploy') }}</el-button
                        >
                        <el-button
                            type="primary"
                            @click="editItem(scope.row)"
                            >{{ $t('message.edit') }}</el-button
                        >
                        <el-button
                            type="primary"
                            @click="deleteItem(scope.row, scope.$index)"
                            >{{ $t('message.delete') }}</el-button
                        >
                        <el-button
                            type="primary"
                            @click="listOfItem(scope.row)"
                            >{{ $t('message.deployedWorkerList') }}</el-button
                        >
                    </template>
                </el-table-column>
            </el-table>
        </el-card> -->

        <el-dialog
            :title="$t('message.newContainer')"
            :visible.sync="dialogVisible"
            width="50%"
            :close-on-click-modal="false"
            v-on:close="closeEdit"
        >
            <el-form
                ref="form"
                :model="form"
                label-width="150px"
                class="genTable"
                label-position="left"
            >
                <el-form-item :label="$t('message.containerName')">
                    <el-input v-model="form.containerName"></el-input>
                </el-form-item>

                <el-form-item :label="'容器描述'">
                    
                    <el-input
                        type="textarea"
                        :rows="3"
                        v-model="form.containerDesc"
                    />
                     
                </el-form-item>

                <el-form-item :label="$t('message.containerType')">
                    <el-radio-group v-model="form.sourceType">
                        <el-radio label="Git"></el-radio>
                        <el-radio label="FatJar"></el-radio>
                        <el-radio label="Script"></el-radio>
                        <el-radio label="Config"></el-radio>
                    </el-radio-group>
                </el-form-item>
                <el-form
                    v-if="form.sourceType == 'Git'"
                    ref="gitform"
                    :model="gitForm"
                    label-width="150px"
                    class="gitTable"
                    label-position="left"
                >
                    <el-form-item :label="$t('message.containerGitURL')">
                        <el-input v-model="gitForm.repo"></el-input>
                    </el-form-item>
                    <el-form-item :label="$t('message.branchName')">
                        <el-input v-model="gitForm.branch"></el-input>
                    </el-form-item>
                    <el-form-item :label="$t('message.username')">
                        <el-input v-model="gitForm.username"></el-input>
                    </el-form-item>
                    <el-form-item :label="$t('message.password')">
                        <el-input v-model="gitForm.password"></el-input>
                    </el-form-item>
                </el-form>
                <el-form-item v-show="form.sourceType == 'FatJar'">
                    <el-upload
                        class="upload-demo"
                        drag
                        :http-request="changeFileName"
                        :file-list="fileList"
                        :on-progress="handleUploading"
                        :on-success="onSuccess"
                        :action="`${requestUrl}/container/jarUpload`"
                        multiple
                    >
                        <i class="el-icon-upload"></i>
                        <div class="el-upload__text">
                            Drag the file here, or <em>click on Upload</em>
                        </div>
                        <div class="el-upload__tip" slot="tip">
                            {{ $t('message.uploadTips') }}
                        </div>
                    </el-upload>
                </el-form-item>
                <el-form-item v-show="form.sourceType == 'Script'">
                    <el-upload
                        class="upload-demo"
                        drag
                        :http-request="changeFileName"
                        :file-list="fileList"
                        :on-success="onSuccessScript"
                        :action="`${requestUrl}/container/scriptUpload`"
                        multiple
                    >
                        <i class="el-icon-upload"></i>
                        <div class="el-upload__text">
                            Drag the file here, or <em>click on Upload</em>
                        </div>
                        <div class="el-upload__tip" slot="tip">
                            {{ $t('message.uploadTips') }}
                        </div>
                    </el-upload>
                </el-form-item>
                <el-form-item v-show="form.sourceType == 'Config'">
                    <el-upload
                        class="upload-demo"
                        drag
                        :http-request="changeFileName"
                        :file-list="fileList"
                        :on-success="onSuccessConfig"
                        :action="`${requestUrl}/container/scriptUpload`"
                        multiple
                    >
                        <i class="el-icon-upload"></i>
                        <div class="el-upload__text">
                            Drag the file here, or <em>click on Upload</em>
                        </div>
                        <div class="el-upload__tip" slot="tip">
                            {{ $t('message.uploadTips') }}
                        </div>
                    </el-upload>
                </el-form-item>
                <el-form-item :label="'文件路径'" v-if="showPath">
                     <el-input v-model="form.containerExecPath" :placeholder="'压缩包里执行文件路径'"></el-input>
                </el-form-item>

                <el-form-item>
                    <el-button
                        type="primary"
                        @click="onSubmit"
                        :disabled="
                            ((form.sourceType == 'FatJar' && !this.sourceInfo) ||
                                (form.sourceType == 'Script' && 
                                    !this.sourceInfo) || (form.sourceType == 'Config' && 
                                    !this.sourceInfo)) && this.editFlag
                        "
                        >Save</el-button
                    >
                </el-form-item>
            </el-form>
        </el-dialog>
        <el-dialog
            :title="arrangeTitle"
            :visible.sync="arrangeVisible"
            v-on:close="closeArrange"
        >
            <h4 v-for="log in logs" :key="log">{{ log }}</h4>
        </el-dialog>
    </div>
</template>

<script>
import baseURL from '../../main';
import baseUrl from '../../main';
import axios from 'axios';
let ws;
export default {
    name: 'ContainerManager',
    data() {
        return {
            form: {
                sourceType: 'Git',
                containerDesc:'',
                containerName: '',
                containerExecPath: ''
            },
            gitForm: {
                repo: '',
                branch: '',
                username: '',
                password: ''
            },
            // 任务查询请求对象
            containerQueryContent: {
                appId: this.$store.state.appInfo.id,
                index: 0,
                pageSize: 10,
                sourceType: undefined,
                keyword: undefined
            },
            // 任务列表（查询结果），包含index、pageSize、totalPages、totalItems、data（List类型）
            containerPageResult: {
                pageSize: 10,
                totalItems: 0,
                data: []
            },
            current:1,
            sourceInfo: '',
            editFlag: true,
            showPath: false,
            id: '',
            appId: this.$store.state.appInfo.id,
            dialogVisible: false,
            arrangeTitle: '',
            arrangeVisible: false,
            containerList: [],
            logs: [],
            requestUrl: '',
            fileList: [],
            sourceTypeOptions: [
                    {key: null, label: this.$t('message.all')},
                    {key: "Git", label: 'Git'},
                    {key: "FatJar", label: 'FatJar'},
                    {key: "Script", label: 'Script'},
                    {key: "Config", label: 'Config'}
                ]
        };
    },
    methods: {
        handleUploading() {},
        changeFileName(fileObj) {
            let newFile = new File(
                [fileObj.file],
                fileObj.file.name + '.goldwind'
            );
            let param = new FormData(); //创建form对象

            param.append('file', newFile); //通过append向form对象添加数据

            return axios
                .request({
                    url: fileObj.action,
                    method: 'post',
                    data: param,
                    timeout: 3600000,
                    onUploadProgress: progressEvent => {
                        const complete =
                            ((progressEvent.loaded / progressEvent.total) *
                                100) |
                            0;
                        fileObj.onProgress({ percent: complete });
                    }
                })
                .then(this.handleUpload, this.handleUploadError);
        },
        listContainer(){
            //let appId = this.$store.state.appInfo.id;
            this.axios.post('/container/listPage', this.containerQueryContent).then(res => {
                // 刷新容器表单
                //this.containerList = res;
                this.containerPageResult = res;
            });
        },
        onClickReset() {
            this.containerQueryContent.keyword = undefined;
            this.containerQueryContent.sourceType = undefined;
            this.containerQueryContent.index = 0;
            this.listContainer();
            this.current = 1;
        },
        // 点击 换页
        onClickChangePage(index) {
            // 后端从0开始，前端从1开始
            this.containerQueryContent.index = index - 1;
            this.listContainer();
        },
        onClickNewContainer(){
            this.resetForm();
            this.form.sourceType = 'Script';
            this.dialogVisible = true;
        },
        resetForm(){
            this.form.containerName = undefined;
            this.form.containerDesc = undefined;
            this.form.containerExecPath = undefined;
            this.id = undefined;
            this.showPath = false;
        },
        onSubmit() {
            // 接口参数
            let data = {
                appId: this.appId,
                containerName: this.form.containerName,
                containerDesc: this.form.containerDesc,
                status: 'ENABLE',
                id: this.id,
                sourceType: this.form.sourceType,
                containerExecPath: this.form.containerExecPath
            };
            if (this.form.sourceType == 'Git') {
                data.sourceInfo = JSON.stringify(this.gitForm);
            } else if (this.form.sourceType == 'FatJar') {
                data.sourceInfo = this.sourceInfo;
                data.sourceType = 'FatJar';
            } else if (this.form.sourceType == 'Script'){
                data.sourceInfo = this.sourceInfo;
                data.sourceType = 'Script';
            } else{
                data.sourceInfo = this.sourceInfo;
                data.sourceType = 'Config';
            }

         
            
            this.axios.post('container/save', data).then(() => {
                this.listContainer();
                // 恢复默认表单
                this.dialogVisible = false;
                this.resetForm();
                this.gitForm = {};
                this.sourceInfo = '';
                this.id = '';
                    // 刷新容器表单
                // let appId = this.$store.state.appInfo.id;
                // this.axios.get('/container/list?appId=' + appId).then(res => {
                //     this.$message.info(this.$t('message.success'));
                //     // 恢复默认表单
                //     this.dialogVisible = false;
                //     this.form.containerName = '';
                //     this.gitForm = {};
                //     this.sourceInfo = '';
                //     this.id = '';
                //     // 刷新容器表单
                //     this.containerList = res;
                // });
            });
        },
        // 文件上传成功后 修改来源信息
        onSuccess(response) {
            this.sourceInfo = response;
        },
        onSuccessScript(response){
            this.sourceInfo = response;
            let suffix = response.substring(response.lastIndexOf(".")+1);
            let compressList = ['zip','rar','gz'];
            this.showPath = compressList.some(function(item){
                return suffix == item;  
            })
        },
        onSuccessConfig(response){
            this.onSuccessScript(response);
            this.form.containerExecPath = '/parameter.json';
        },
        deleteItem(item, index) {
            console.log(item, index);
            let appId = this.$store.state.appInfo.id;
            this.flyio
                .get(
                    '/container/delete?containerId=' +
                        item.id +
                        '&appId=' +
                        appId
                )
                .then(res => {
                    console.log(res);
                    this.containerPageResult.data.splice(index, 1);
                    this.$message.info(this.$t('message.success'));
                });
        },
        editItem(item) {
            if (item.sourceType == 'Git') {
                this.form.sourceType = 'Git';
                this.gitForm = JSON.parse(item.sourceInfo);
            } else if (item.sourceType == 'FatJar') {
                this.form.sourceType = 'FatJar';
            } else if (item.sourceType == 'Script') {
                this.form.sourceType = 'Script';
            } else{
                this.form.sourceType = 'Config';
            }
            this.form.containerName = item.containerName;
            this.form.containerDesc = item.containerDesc;
            let execPath = item.containerExecPath;
            if(execPath !='' && execPath != undefined ){
                this.form.containerExecPath = execPath;
                this.showPath = true;
            }
            this.id = item.id;
            this.editFlag = false;
            this.dialogVisible = true;
        },
        arrangeItem(item) {
            let wsBase = baseURL.replace('http', 'ws') + '/container/deploy/';
            let wsUrl = wsBase + item.id;
            ws = new WebSocket(wsUrl);

            ws.onopen = () => {
                this.arrangeTitle = this.$t('message.deploy');
                this.arrangeVisible = true;
                console.log('Connection open ...');
                ws.send('Hello WebSockets!');
            };

            ws.onmessage = evt => {
                this.logs.push(evt.data);
            };

            ws.onclose = () => {
                console.log('Connection closed.');
            };
        },
        // 关闭部署页面时 关闭ws避免dialog内的信息有上台机器信息
        closeArrange() {
            ws.close();
            this.logs = [];
        },
        closeEdit() {
            this.sourceInfo = '';
            this.dialogVisible = false;
            this.resetForm();
            this.gitForm = {};
            this.fileList = [];
        },
        listOfItem(item) {
            let appId = this.$store.state.appInfo.id;
            this.flyio
                .get(
                    '/container/listDeployedWorker?containerId=' +
                        item.id +
                        '&appId=' +
                        appId
                )
                .then(res => {
                    if (res.data.data) {
                        this.logs = res.data.data.split('\n');
                        this.arrangeTitle = this.$t(
                            'message.deployedWorkerList'
                        );
                        this.arrangeVisible = true;
                    }
                    // this.containerList.splice(index,1);
                    // this.$message(`容器${item.containerName}已删除`);
                });
        },
        // 兼容 java build in 模式下 baseURL 为 / 的情况（将当前url作为请求路径）
        calculateRequestUrl() {
            if (baseUrl === undefined || !baseUrl.includes('http')) {
                let url = window.location.href;
                let urlSplit = url.split('//'); // str1[0]--协议头
                let ip = urlSplit[1].split('/')[0];
                this.requestUrl = urlSplit[0] + '//' + ip;
                console.log('calculateRequestUrl: ' + this.requestUrl);
            } else {
                this.requestUrl = baseUrl;
            }
        }
    },
    mounted() {
        this.calculateRequestUrl();

        // let appId = this.$store.state.appInfo.id;
        this.flyio.post('/container/listPage', this.containerQueryContent).then(res => {
            console.log(res);
            if (res.data.success) {
                this.containerPageResult = res.data.data;
            }
        });
    }
};
</script>

<style scoped>
.genTable {
    padding: 20px;
    min-width: 500px;
    width: 500px;
}
.clearfix:before,
.clearfix:after {
    display: table;
    content: '';
}
.clearfix:after {
    clear: both;
}
.wrapper {
    display: flex;
    flex-wrap: wrap;
}
.item {
    flex: 0 0 340px;
    margin-right: 20px;
    margin-bottom: 20px;
    background-color: #f0f0f0;
}
.item button {
    width: 100px;
    margin: 0 auto;
}
.btnWrap {
    width: 50%;
    float: left;
    margin-bottom: 20px;
    display: flex;
    justify-content: center;
}
.containerText {
    margin: 20px;
    font-size: 16px;
    box-sizing: border-box;
}
.value {
    display: inline-block;
    max-width: 200px;
    overflow: hidden;
    margin-left: 20px;
}
.el-dialog {
    height: 100vh;
}
</style>
