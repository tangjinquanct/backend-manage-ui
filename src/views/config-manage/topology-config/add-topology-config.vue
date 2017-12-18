<template>
    <Card>
        <p slot="title">
            <Icon type="android-more-horizontal"></Icon>
            添加Topology配置
        </p>
        <Form ref="rowData" :model="rowData" :label-width="150" :rules="ruleValidate">
            <FormItem label="topologyName" prop="topologyName">
                <Input v-model="rowData.topologyName" placeholder="请输入" value="rowData.topologyName"></Input>
            </FormItem>
            <FormItem label="topic" prop="topic">
                <Input v-model="rowData.topic" placeholder="请输入" value="rowData.topic"></Input>
            </FormItem>
            <FormItem label="topicCluster" prop="topicCluster">
                <Input v-model="rowData.topicCluster" placeholder="请输入" value="rowData.topicCluster" ></Input>
            </FormItem>
            <FormItem label="workerNum" prop="workerNum">
                <Input v-model="rowData.workerNum" placeholder="请输入" value="rowData.workerNum" number></Input>
            </FormItem>
            <FormItem label="workerMemGb" prop="workerMemGb">
                <Input v-model="rowData.workerMemGb" placeholder="请输入" value="rowData.workerMemGb" number></Input>
            </FormItem>
            <FormItem label="spoutNum" prop="spoutNum">
                <Input v-model="rowData.spoutNum" placeholder="请输入" value="rowData.spoutNum" number></Input>
            </FormItem>
            <FormItem label="sleepMs" prop="sleepMs">
                <Input v-model="rowData.sleepMs" placeholder="请输入" value="rowData.sleepMs" number></Input>
            </FormItem>
            <FormItem label="dispatcherBoltNum" prop="dispatcherBoltNum">
                <Input v-model="rowData.dispatcherBoltNum" placeholder="请输入" value="rowData.dispatcherBoltNum" number></Input>
            </FormItem>
            <FormItem label="esBoltNum" prop="esBoltNum">
                <Input v-model="rowData.esBoltNum" placeholder="请输入" value="rowData.esBoltNum" number></Input>
            </FormItem>
            <FormItem label="mysqlBoltNum" prop="mysqlBoltNum">
                <Input v-model="rowData.mysqlBoltNum" placeholder="请输入" value="rowData.mysqlBoltNum" number></Input>
            </FormItem>
            <FormItem label="memsqlBoltNum" prop="memsqlBoltNum">
                <Input v-model="rowData.memsqlBoltNum" placeholder="请输入" value="rowData.memsqlBoltNum" number></Input>
            </FormItem>
            <FormItem label="clickHouseBoltNum" prop="clickHouseBoltNum">
                <Input v-model="rowData.clickHouseBoltNum" placeholder="请输入" value="rowData.clickHouseBoltNum" number></Input>
            </FormItem>
            <FormItem>
                <Button type="primary" @click="doAddData">保存</Button>
                <Button type="ghost" style="margin-left: 8px" @click="cancel">取消</Button>
                <Button type="ghost" style="margin-left: 8px" @click="resetCheck">重置</Button>
            </FormItem>
        </Form>
    </Card>
</template>

<script>
    export default {
        data () {
            let This = this;
            // const validate = (rule, value, callback) => {
            //     if (!Number.isInteger(value)) {
            //         callback(new Error('请输入整数'));
            //     } else if (!This.rowData.boltNum && !This.rowData.bufferSizeMb && !This.rowData.bufferSeconds) {
            //         callback(new Error('boltNum ,bufferSizeMb,bufferSeconds 不能同时为空'));
            //     } else {
            //         callback();
            //     }
            // };
            const validateBoltNum = (rule, value, callback) => {
                // if (!Number.isInteger(value)) {
                //     callback(new Error('请输入整数'));
                // }
                if (!This.rowData.esBoltNum && !This.rowData.mysqlBoltNum && !This.rowData.memsqlBoltNum && !This.rowData.clickHouseBoltNum) {
                    callback(new Error('esBoltNum,mysqlBoltNum,memsqlBoltNum,clickHouseBoltNum 不能同时为空'));
                } else {
                    callback();
                }
            };
            return {
                rowData: {},
                ruleValidate: {
                    topologyName: [
                        {required: true, type: 'string', max: 256, message: '请输入字符串[1-256字符]', trigger: 'blur'}
                    ],
                    topic: [
                        {required: true, type: 'string', max: 256, message: '请输入字符串[1-256字符]', trigger: 'blur'}
                    ],
                    topicCluster: [
                        {required: true, type: 'string', max: 256, message: '请输入字符串[1-256字符]', trigger: 'blur'}
                    ],
                    workerNum: [
                        {required: true, type: 'integer', min: 1, message: '请输入大于零的整数', trigger: 'blur'}
                    ],
                    workerMemGb: [
                        {required: true, type: 'integer', min: 1, message: '请输入大于零的整数', trigger: 'blur'}
                    ],
                    spoutNum: [
                        {required: true, type: 'integer', min: 1, message: '请输入大于零的整数', trigger: 'blur'}
                    ],
                    sleepMs: [
                        // {required: true, message: 'bufferSizeMb cannot be empty', trigger: 'blur'},
                        {type: 'integer', min: 1, message: '请输入大于零的整数', trigger: 'blur'}
                    ],
                    dispatcherBoltNum: [
                        {required: true, type: 'integer', min: 1, message: '请输入大于零的整数', trigger: 'blur'}
                    ],
                    esBoltNum: [
                        // {required: false, message: 'bufferSizeMb cannot be empty', trigger: 'blur'},
                        {type: 'integer', min: 1, message: '请输入大于零的整数', trigger: 'blur'},
                        {validator: validateBoltNum, trigger: 'blur'}

                    ],
                    mysqlBoltNum: [
                        // {required: false, message: 'bufferSizeMb cannot be empty', trigger: 'blur'},
                        {type: 'integer', min: 1, message: '请输入大于零的整数', trigger: 'blur'},
                        {validator: validateBoltNum, trigger: 'blur'}
                    ],
                    memsqlBoltNum: [
                        // {required: false, message: 'bufferSizeMb cannot be empty', trigger: 'blur'},
                        {type: 'integer', min: 1, message: '请输入大于零的整数', trigger: 'blur'},
                        {validator: validateBoltNum, trigger: 'blur'}
                    ],
                    clickHouseBoltNum: [
                        // {required: false, message: 'bufferSizeMb cannot be empty', trigger: 'blur'},
                        {type: 'integer', min: 1, message: '请输入大于零的整数', trigger: 'blur'},
                        {validator: validateBoltNum, trigger: 'blur'}
                    ]
                }
            };
        },
        methods: {
            checkData: function () {
                let This = this;
                let ret = true;
                This.$refs.rowData.validate((valid) => { ret = valid; });
                return ret;
            },
            resetCheck: function () {
                let This = this;
                This.$refs.rowData.resetFields();
                This.rowData = {};
            },
            doAddData: function () {
                let This = this;
                if (This.checkData()) {
                    This.$apiAxios.post('/topologyConfig/add', This.rowData, function (result) {
                        if (result.code === 200) {
                            This.$router.push({name: 'topology-config'});
                        } else {
                            let errorMsg = This.$util.getErrorMsg(result.code);
                            This.$Message.error(errorMsg);
                        }
                    }, function (e) {
                        This.$Message.error(e.statusText);
                    });
                }
            },
            cancel: function () {
                let This = this;
                This.$router.push({name: 'topology-config'});
            }
        }
    };
</script>

