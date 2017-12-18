<style lang="less">
    @import '../../../styles/common.less';
    @import '../../../styles/table.less';
</style>
<template>
    <div>
        <Card>
            <p slot="title">
                <Icon type="android-more-horizontal"></Icon>
                数据配置表（data_config）
            </p>
            <Row class="ivu-card-head">
                <i-col span="6" class="table-form-input-select-col" order="4">
                    <Button type="primary" shape="circle" icon="plus-round" size="large" @click="showAddData">
                        添加数据配置
                    </Button>
                </i-col>
                <i-col span="6" class="table-form-input-select-col" order="4" style="float:right;">
                    <Input icon="ios-search" v-model="searchColName" placeholder="请输入数据名" @on-change="searchData"></Input>
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
        <add-data-config ref="addDataConfig" style="display: none"></add-data-config>
        <Modal v-model="showDetailFlag" title="查看详情">
            <Form :label-width="100">
                <FormItem label="dataName" >
                    {{rowData.dataName}}
                </FormItem>
                <FormItem label="columnName">
                    {{rowData.columnName}}
                </FormItem>
                <FormItem label="boltNum">
                    {{rowData.boltNum}}
                </FormItem>
                <FormItem label="bufferSizeMb">
                    {{rowData.bufferSizeMb}}
                </FormItem>
                <FormItem label="bufferSeconds">
                    {{rowData.bufferSeconds}}
                </FormItem>
                <FormItem label="topic">
                    {{rowData.topic}}
                </FormItem>
                <FormItem label="topicCluster">
                    {{rowData.topicCluster}}
                </FormItem>
                <FormItem label="storeConfig">
                    {{rowData.storeConfig}}
                </FormItem>
            </Form>
        </Modal>

        <!--<Modal v-model="showEditFlag" title="编辑数据配置" @on-ok="doEditData">-->
            <!--<Form :model="rowData" :label-width="100">-->
                <!--<FormItem label="dataName">-->
                    <!--<Input v-model="rowData.dataName" placeholder="请输入" value="rowData.dataName" disabled></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="columnName">-->
                    <!--<Input v-model="rowData.columnName" placeholder="请输入" value="rowData.columnName"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="boltNum">-->
                    <!--<Input v-model="rowData.boltNum" placeholder="请输入" value="rowData.boltNum"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="bufferSizeMb">-->
                    <!--<Input v-model="rowData.bufferSizeMb" placeholder="请输入" value="rowData.bufferSizeMb"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="bufferSeconds">-->
                    <!--<Input v-model="rowData.bufferSeconds" placeholder="请输入" value="rowData.bufferSeconds"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="topic">-->
                    <!--<Input v-model="rowData.topic" placeholder="请输入" value="rowData.topic"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="topicCluster">-->
                    <!--<Input v-model="rowData.topicCluster" placeholder="请输入" value="rowData.topicCluster"></Input>-->
                <!--</FormItem>-->
                <!--<FormItem label="storeConfig">-->
                    <!--<Input v-model="rowData.storeConfig" type="textarea" placeholder="请输入" value="rowData.storeConfig"></Input>-->
                <!--</FormItem>-->
            <!--</Form>-->
        <!--</Modal>-->
    </div>
</template>
<script>
    import addDataConfig from './add-data-config.vue';

    export default {
        components: {addDataConfig},
        data: function () {
            return {

                tableColumns: [
                    {
                        title: 'dataName',
                        key: 'dataName'
                    },
                    {
                        title: 'columnName',
                        key: 'columnName'
                    },
                    {
                        title: 'boltNum',
                        key: 'boltNum'
                    },
                    {
                        title: 'bufferSizeMb',
                        key: 'bufferSizeMb'
                    },
                    {
                        title: 'bufferSeconds',
                        key: 'bufferSeconds'
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
                        title: 'storeConfig',
                        key: 'storeConfig'
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
                                            this.showDetail(params);
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
                                        icon: 'android-delete',

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
                searchColName: '',
                loadingFlag: false
            };
        },
        created () {
            this.getTableDatas();
        },
        methods: {
            getTableDatas () {
                var This = this;
                This.showPageFlag = 'block';
                This.$apiAxios.get('/dataConfig/getDataConfigs', null, function (result) {
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
                var This = this;
                This.pageData.current = current;
                var start = (current - 1) * This.pageData.pageSize;
                var end = current * This.pageData.pageSize;
                This.tableDatas = This.ajaxTableDatas.slice(start, end);
            },
            pageSizeChange (pageSize) {
                var This = this;
                This.pageData.pageSize = pageSize;
                This.tableDatas = This.ajaxTableDatas.slice(0, pageSize);
            },

            showDetail (rowData) {
                var This = this;
                This.showDetailFlag = true;
                This.rowData = rowData.row;
            },
            showAddData () {
                var This = this;
                This.$router.push({name: 'add-data-config'});
                // This.showAddFlag = true;
                // This.rowData={};
                // var addDataConfigRef = This.$refs.addDataConfig;
                // This.$Modal.confirm({
                //   title:'添加',
                //   loading:This.loadingFlag,
                //   render: (h) => {
                //     return h(addDataConfig, {
                //       props: {
                //
                //       },
                //       on: {
                //
                //       }
                //     })
                //   },
                //   onOk:function(){
                //     var ret = addDataConfigRef.checkData();
                //     if(!ret){
                //       This.loadingFlag=false;
                //       this.$Message.error('请检查表单是否输入正确!');
                //       setTimeout(() => {
                //         This.loadingFlag=true;
                //       }, 100);
                //     }else{
                //       addDataConfigRef.doAddData();
                //       addDataConfigRef.resetCheck();
                //     }
                //   },
                //   onCancel:function () {
                //     addDataConfigRef.resetCheck();
                //   }
                // });
            },
            // doAddData() {
            //     var This = this;
            //     This.$api.post('/dataConfig/addDataConfig', This.rowData, function (result) {
            //         This.getTableDatas();
            //     });
            //     This.rowData = {};
            // },
            showEditData (rowData) {
                // var This = this;
                // This.showEditFlag = true;
                // This.rowData = Object.assign({}, rowData.row);
                var This = this;
                This.$router.push({name: 'edit-data-config', params: { rowData: rowData.row }});
            },
            doEditData () {
                var This = this;
                This.$api.post('/dataConfig/editDataConfig', This.rowData, function (result) {
                    This.getTableDatas();
                });
            },
            deleteData: function (rowData) {
                var This = this;
                This.$apiAxios.post('/dataConfig/deleteDataConfig', rowData.row, function (result) {
                    This.getTableDatas();
                });
            },

            searchData () {
                var This = this;

                if (This.searchColName === '') {
                    This.getTableDatas();
                } else {
                    This.showPageFlag = 'none';
                    This.tableDatas = This.search(this.ajaxTableDatas, {dataName: This.searchColName});
                }
            },
            search: function (data, argumentObj) {
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
    };
</script>
