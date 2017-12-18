<template>
    <Card>
        <p slot="title">
            <Icon type="android-more-horizontal"></Icon>
            编辑数据配置
        </p>
        <Form ref="rowData" :model="rowData" :label-width="100" :rules="ruleValidate">
            <FormItem label="dataName" prop="dataName">
                <Input v-model="rowData.dataName" placeholder="请输入" value="rowData.dataName" disabled></Input>
            </FormItem>
            <FormItem label="columnName" prop="columnName">
                <Input v-model="rowData.columnName" placeholder="请输入" value="rowData.columnName"></Input>
            </FormItem>
            <FormItem label="boltNum" prop="boltNum">
                <Input v-model="rowData.boltNum" placeholder="请输入" value="rowData.boltNum" number></Input>
            </FormItem>
            <FormItem label="bufferSizeMb" prop="bufferSizeMb">
                <Input v-model="rowData.bufferSizeMb" placeholder="请输入" value="rowData.bufferSizeMb" number></Input>
            </FormItem>
            <FormItem label="bufferSeconds" prop="bufferSeconds">
                <Input v-model="rowData.bufferSeconds" placeholder="请输入" value="rowData.bufferSeconds" number></Input>
            </FormItem>
            <FormItem label="topic" prop="topic">
                <Input v-model="rowData.topic" placeholder="请输入" value="rowData.topic"></Input>
            </FormItem>
            <FormItem label="集群topic-topicCluster" prop="topicCluster">
                <Input v-model="rowData.topicCluster" placeholder="请输入" value="rowData.topicCluster"></Input>
            </FormItem>
            <FormItem label="存储配置-storeConfig" prop="storeConfig">
                <Button type="primary" @click="">选项式添加存储配置</Button>
                <Input v-model="rowData.storeConfig" type="textarea" placeholder="请输入" value="rowData.storeConfig" :rows="10"></Input>
            </FormItem>
            <FormItem>
                <Button type="primary" @click="doEditData">保存</Button>
                <Button type="ghost" style="margin-left: 8px" @click="cancel">取消</Button>
                <Button type="ghost" style="margin-left: 8px" @click="resetCheck">重置</Button>
            </FormItem>
        </Form>
    </Card>
</template>

<script>
    export default {
        name: 'edit-config-page',
        data () {
            // var This = this;
            // const validate = (rule, value, callback) => {
            //     if (!Number.isInteger(value)) {
            //         callback(new Error('请输入整数'));
            //     } else if (!This.rowData.boltNum && !This.rowData.bufferSizeMb && !This.rowData.bufferSeconds) {
            //         callback(new Error('boltNum ,bufferSizeMb,bufferSeconds 不能同时为空'));
            //     } else {
            //         callback();
            //     }
            // };
            // const validateDataName = (rule, value, callback) => {
            //     if (!Number.isInteger(value)) {
            //         callback(new Error('请输入整数'));
            //     } else if (!This.rowData.boltNum && !This.rowData.bufferSizeMb && !This.rowData.bufferSeconds) {
            //         callback(new Error('boltNum ,bufferSizeMb,bufferSeconds 不能同时为空'));
            //     } else {
            //         callback();
            //     }
            // };
            return {
                rowData: {},
                ruleValidate: {
                    dataName: [
                        {required: true, message: 'dataName cannot be empty', trigger: 'blur'},
                        {type: 'string', max: 256, message: 'more than 256 words', trigger: 'blur'},
                        // {validator: validateDataName, trigger: 'blur'}
                    ],
                    columnName: [
                        {required: true, message: 'columnName cannot be empty', trigger: 'blur'},
                        {type: 'string', max: 256, message: 'more than 256 words', trigger: 'blur'}
                    ],
                    boltNum: [
                        // {required: false, message: 'boltNum cannot be empty', trigger: 'blur'},
                        {type: 'integer', message: 'Introduce no less than 20 words', trigger: 'blur'}
                    ],
                    bufferSizeMb: [
                        // {required: false, message: 'bufferSizeMb cannot be empty', trigger: 'blur'},
                        {type: 'integer', message: 'Introduce no less than 20 words', trigger: 'blur'}
                    ],
                    bufferSeconds: [
                        // {required: false, message: 'bufferSeconds cannot be empty', trigger: 'blur'},
                        {type: 'integer', message: 'Introduce no less than 20 words', trigger: 'blur'}
                    ],
                    topic: [
                        {required: true, message: 'topic cannot be empty', trigger: 'blur'},
                        {type: 'string', max: 256, message: 'more than 256 words', trigger: 'blur'}
                    ],
                    topicCluster: [
                        {required: true, message: 'topicCluster cannot be empty', trigger: 'blur'},
                        {type: 'string', max: 256, message: 'more than 256 words', trigger: 'blur'}
                    ],
                    storeConfig: [
                        {required: true, message: 'storeConfig cannot be empty', trigger: 'blur'},
                        {type: 'string', max: 10240, message: 'more than 10240 words', trigger: 'blur'}
                    ]
                }
            };
        },
        methods: {
            checkData: function () {
                var This = this;
                var ret = true;
                This.$refs.rowData.validate((valid) => { ret = valid; });
                return ret;
            },
            resetCheck: function () {
                var This = this;
                This.$refs.rowData.resetFields();
                This.rowData = This.$route.params.rowData;
            },
            doEditData: function () {
                var This = this;
                if (This.checkData()) {
                    This.$apiAxios.post('/dataConfig/editDataConfig', This.rowData, function (result) {
                        if (result.code === 200) {
                            This.$router.push({name: 'data-config'});
                        } else {
                            var errorMsg = This.$util.getErrorMsg(result.code);
                            This.$Message.error(errorMsg);
                        }
                    }, function (e) {
                        This.$Message.error(e.statusText);
                    });
                }
            },
            cancel: function () {
                var This = this;
                This.$router.push({name: 'data-config'});
            }
        },
        created () {
            let This = this;
            This.rowData = This.$route.params.rowData;
        }
    };
</script>

