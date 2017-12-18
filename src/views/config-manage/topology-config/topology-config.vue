<template>
    <div>
        <Card>
            <p slot="title">
                <Icon type="android-more-horizontal"></Icon>
                Topology配置表（topology_config）
            </p>
            <Row class="ivu-card-head">
                <i-col span="6" class="table-form-input-select-col" order="4">
                    <Button type="primary" shape="circle" icon="plus-round" size="large" @click="showAddData">
                        添加Topology配置
                    </Button>
                </i-col>
                <i-col span="6" class="table-form-input-select-col" order="4" style="float:right;">
                    <Input icon="ios-search" v-model="searchColName" placeholder="请输入Topology名" @on-change="searchData"></Input>
                </i-col>
            </Row>

            <Row >

                <Table border :columns="tableColumns" :data="tableDatas"></Table>
            </Row>
            <Row  align="center" v-bind:style="{display:showPageFlag}">
                <Page @on-change="pageChange" @on-page-size-change="pageSizeChange" :total="pageData.total"
                      :current="pageData.current" :page-size="pageData.pageSize" style="padding:5px 5px 2px 5px;"
                      show-elevator show-sizer show-total></Page>
            </Row>
        </Card>

        <Modal v-model="showDetailFlag" title="查看详情">
            <Form :label-width="150">
                <FormItem label="topologyName:">
                    {{rowData.topologyName}}
                </FormItem>
                <FormItem label="topic:">
                    {{rowData.topic}}
                </FormItem>
                <FormItem label="topicCluster:">
                    {{rowData.topicCluster}}
                </FormItem>
                <FormItem label="workerNum:">
                    {{rowData.workerNum}}
                </FormItem>
                <FormItem label="workerMemGb:">
                    {{rowData.workerMemGb}}
                </FormItem>
                <FormItem label="spoutNum:">
                    {{rowData.spoutNum}}
                </FormItem>
                <FormItem label="sleepMs: ">
                    {{rowData.sleepMs}}
                </FormItem>
                <FormItem label="dispatcherBoltNum:">
                    {{rowData.dispatcherBoltNum}}
                </FormItem>
                <FormItem label="esBoltNum:">
                    {{rowData.esBoltNum}}
                </FormItem>
                <FormItem label="mysqlBoltNum: ">
                    {{rowData.mysqlBoltNum}}
                </FormItem>
                <FormItem label="memsqlBoltNum:">
                    {{rowData.memsqlBoltNum}}
                </FormItem>
                <FormItem label="clickHouseBoltNum:">
                    {{rowData.clickHouseBoltNum}}
                </FormItem>
            </Form>
        </Modal>
        <!--<Modal v-model="showAddFlag" title="添加Topology配置" @on-ok="doAddData">-->
            <!--<Form :model="rowData" :label-width="100">-->
                <!--<FormItem label="topologyName">-->
                    <!--<Input v-model="rowData.topologyName" placeholder="请输入" value="rowData.topologyName"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="topic">-->
                    <!--<Input v-model="rowData.topic" placeholder="请输入" value="rowData.topic"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="topicCluster">-->
                    <!--<Input v-model="rowData.topicCluster" placeholder="请输入" value="rowData.topicCluster"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="workerNum">-->
                    <!--<Input v-model="rowData.workerNum" placeholder="请输入" value="rowData.workerNum"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="workerMemGb">-->
                    <!--<Input v-model="rowData.workerMemGb" placeholder="请输入" value="rowData.workerMemGb"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="spoutNum">-->
                    <!--<Input v-model="rowData.spoutNum" placeholder="请输入" value="rowData.spoutNum"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="sleepMs">-->
                    <!--<Input v-model="rowData.sleepMs" placeholder="请输入" value="rowData.sleepMs"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="dispatcherBoltNum">-->
                    <!--<Input v-model="rowData.dispatcherBoltNum" placeholder="请输入"-->
                           <!--value="rowData.dispatcherBoltNum"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="esBoltNum">-->
                    <!--<Input v-model="rowData.esBoltNum" placeholder="请输入" value="rowData.esBoltNum"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="mysqlBoltNum">-->
                    <!--<Input v-model="rowData.mysqlBoltNum" placeholder="请输入" value="rowData.mysqlBoltNum"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="memsqlBoltNum">-->
                    <!--<Input v-model="rowData.memsqlBoltNum" placeholder="请输入" value="rowData.memsqlBoltNum"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="clickHouseBoltNum">-->
                    <!--<Input v-model="rowData.clickHouseBoltNum" placeholder="请输入"-->
                           <!--value="rowData.clickHouseBoltNum"></Input>-->
                <!--</FormItem>-->
            <!--</Form>-->
        <!--</Modal>-->
        <!--<Modal v-model="showEditFlag" title="编辑Topology配置" @on-ok="doEditData">-->
            <!--<Form :model="rowData" :label-width="100">-->
                <!--<FormItem label="dataName">-->
                    <!--<Input v-model="rowData.topologyName" placeholder="请输入" value="rowData.topologyName"-->
                           <!--disabled></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="topic">-->
                    <!--<Input v-model="rowData.topic" placeholder="请输入" value="rowData.topic"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="topicCluster">-->
                    <!--<Input v-model="rowData.topicCluster" placeholder="请输入" value="rowData.topicCluster"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="workerNum">-->
                    <!--<Input v-model="rowData.workerNum" placeholder="请输入" value="rowData.workerNum"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="workerMemGb">-->
                    <!--<Input v-model="rowData.workerMemGb" placeholder="请输入" value="rowData.workerMemGb"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="spoutNum">-->
                    <!--<Input v-model="rowData.spoutNum" placeholder="请输入" value="rowData.spoutNum"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="sleepMs">-->
                    <!--<Input v-model="rowData.sleepMs" placeholder="请输入" value="rowData.sleepMs"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="dispatcherBoltNum">-->
                    <!--<Input v-model="rowData.dispatcherBoltNum" placeholder="请输入"-->
                           <!--value="rowData.dispatcherBoltNum"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="esBoltNum">-->
                    <!--<Input v-model="rowData.esBoltNum" placeholder="请输入" value="rowData.esBoltNum"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="mysqlBoltNum">-->
                    <!--<Input v-model="rowData.mysqlBoltNum" placeholder="请输入" value="rowData.mysqlBoltNum"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="memsqlBoltNum">-->
                    <!--<Input v-model="rowData.memsqlBoltNum" placeholder="请输入" value="rowData.memsqlBoltNum"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="clickHouseBoltNum">-->
                    <!--<Input v-model="rowData.clickHouseBoltNum" placeholder="请输入"-->
                           <!--value="rowData.clickHouseBoltNum"></Input>-->
                <!--</FormItem>-->
            <!--</Form>-->
        <!--</Modal>-->
    </div>
</template>
<script>
    export default {
        data () {
            return {

                tableColumns: [
                    {
                        title: 'topologyName',
                        key: 'topologyName'
                    },
                    {
                        title: 'topic',
                        key: 'topic'
                    },
                    {
                        title: 'topicCluster',
                        key: 'topicCluster'
                    },
                    {
                        title: 'workerNum',
                        key: 'workerNum'
                    },
                    {
                        title: 'workerMemGb',
                        key: 'workerMemGb'
                    },
                    {
                        title: 'spoutNum',
                        key: 'spoutNum'
                    },
                    {
                        title: 'sleepMs',
                        key: 'sleepMs'
                    },
                    {
                        title: 'dispatcherBoltNum',
                        key: 'dispatcherBoltNum'
                    },
                    {
                        title: 'esBoltNum',
                        key: 'esBoltNum'
                    },
                    {
                        title: 'mysqlBoltNum',
                        key: 'mysqlBoltNum'
                    },
                    {
                        title: 'memsqlBoltNum',
                        key: 'memsqlBoltNum'
                    },
                    {
                        title: 'clickHouseBoltNum',
                        key: 'clickHouseBoltNum'
                    },
                    {
                        title: '操作',
                        key: 'action',
                        width: 150,
                        render: (h, params) => {
                            return h('div', [
                                h('Button', {
                                    props: {
                                        type: 'info',
                                        size: 'small',
                                        icon: 'eye',
                                        vertical: 'text-top'
                                    },
                                    title: 'test',
                                    style: {
                                        marginRight: '5px'
                                    },
                                    on: {
                                        click: () => {
                                            this.showDetail(params)
                                        }
                                    }
                                }),
                                h('Button', {
                                    props: {
                                        type: 'info',
                                        size: 'small',
                                        icon: 'edit'
                                    },
                                    style: {
                                        marginRight: '5px'
                                    },
                                    on: {
                                        click: () => {
                                            this.showEditData(params);
                                        }
                                    }
                                }),
                                h('Button', {
                                    props: {
                                        type: 'error',
                                        size: 'small',
                                        icon: 'android-delete'

                                    },
                                    on: {
                                        click: () => {
                                            var This = this;
                                            This.$Modal.confirm({
                                                title: '确认对话框',
                                                content: '确认删除吗?',
                                                onOk: function () {
                                                    This.deleteData(params);
                                                },
                                                onCancel: function () {

                                                }
                                            });
                                        }
                                    }
                                })
                            ]);
                        }
                    }
                ],
                tableDatas: [],
                ajaxTableDatas: [],
                rowData: {},
                pageData: {
                    current: 1,
                    total: 0,
                    pageSize: 10
                },
                showDetailFlag: false,
                showEditFlag: false,
                showAddFlag: false,
                showPageFlag: 'none',
                searchColName: ''
            };
        },
        created () {
            this.getTableDatas();
        },
        methods: {
            getTableDatas () {
                var This = this;
                This.showPageFlag = 'block';
                This.$apiAxios.get('/topologyConfig/queryAll', null, function (result) {
                    This.ajaxTableDatas = result.data;
                    This.pageData.total = This.ajaxTableDatas.length;

                    var start = (This.pageData.current - 1) * This.pageData.pageSize;
                    if (start >= This.pageData.total) {
                        start = (start - This.pageData.pageSize) > 0 ? (start - This.pageData.pageSize) : 0;
                    }
                    var end = start + This.pageData.pageSize;
                    This.tableDatas = This.ajaxTableDatas.slice(start, end);
                });
            },
            pageChange (current) {
                let This = this;
                This.pageData.current = current;
                let start = (current - 1) * This.pageData.pageSize;
                let end = current * This.pageData.pageSize;
                This.tableDatas = This.ajaxTableDatas.slice(start, end);
            },
            pageSizeChange (pageSize) {
                var This = this;
                This.pageData.pageSize = pageSize;
                This.tableDatas = This.ajaxTableDatas.slice(0, pageSize);
            },

            showDetail (rowData) {
                let This = this;
                This.showDetailFlag = true;
                This.rowData = rowData.row;
            },
            showAddData () {
                let This = this;
                // This.showAddFlag = true;
                // This.rowData = {};
                This.$router.push({name: 'add-topology-config'});
            },
            // doAddData () {
            //     var This = this;
            //     This.$api.post('/topologyConfig/addTopologyConfig', This.rowData, function (result) {
            //         This.getTableDatas();
            //     });
            //     This.rowData = {};
            // },
            showEditData (rowData) {
                // var This = this;
                // This.showEditFlag = true;
                // This.rowData = Object.assign({}, rowData.row);
                let This = this;
                This.$router.push({name: 'edit-topology-config', params: { rowData: rowData.row }});
            },
            // doEditData() {
            //     var This = this;
            //     This.$api.post('/topologyConfig/editTopologyConfig', This.rowData, function (result) {
            //         This.getTableDatas();
            //     })
            // },
            deleteData (rowData) {
                var This = this;
                This.$api.post('/topologyConfig/delete', rowData.row, function (result) {
                    This.getTableDatas();
                });
            },

            searchData () {
                var This = this;

                if (This.searchColName === '') {
                    This.getTableDatas();
                } else {
                    This.showPageFlag = 'none';
                    This.tableDatas = This.search(this.ajaxTableDatas, {topologyName: This.searchColName});
                }
            },
            search (data, argumentObj) {
                let res = data;
                let dataClone = data;
                for (let argu in argumentObj) {
                    if (argumentObj[argu].length > 0) {
                        res = dataClone.filter(d => {
                            return d[argu].indexOf(argumentObj[argu]) > -1;
                        });
                        dataClone = res;
                    }
                }
                return res;
            }

        }
    }
</script>
