<template>
  <a-card>
    <table-page ref="tablePage" v-bind="tablePageConfig" :scroll="{ x: true }">
      <template #table-operator>
        <a-button 
          v-has="'serviceAnnualQualityPromise:add'" 
          type="primary" 
          icon="plus" 
          @click="handleAdd" 
          style="margin-left: 20px; 
          color: white;
          width: 80px;">
          新增
        </a-button>
      </template>

<template #index="{ index }">
        {{ index + 1 }}
      </template>
<template #detailStr="{ text }">
        <a-popover>
          <template slot="content">
            {{ text }}
          </template>
<div style="
              cursor: pointer;
              overflow: hidden;
              text-overflow: ellipsis;
              display: -webkit-box;
              -webkit-line-clamp: 1;
              -webkit-box-orient: vertical;
            ">
    {{ text }}
</div>
</a-popover>
</template>

<span slot="operation" slot-scope="{text, record}" @click.stop="">
        <span v-if="record.dataFlag == 0">
          <a-botton style="padding: 0 5px" :disabled="record.updateFlag === 0" type="link"
            v-has="'serviceAnnualQualityPromise:edit'" @click="handleUpDate(text)" class="ant-btn ant-btn-link">
            <a-icon type="details" />编辑
          </a-botton>
          <a-divider type="vertical" />
        </span>

<a @click="handleDetails(text)">
    <a-icon type="details" class="anticon anticon-details" /> 详情
</a>
<span v-if="record.dataFlag == 0">
          <a-divider type="vertical" />
          <a-popconfirm 
            v-has="'serviceAnnualQualityPromise:remove'" 
            title="确定删除吗?"
            @confirm="() => handleDelete(test.id)" 
            role="separator" >
            <a> 删除 </a>
          </a-popconfirm>
        </span>
</span>

</table-page>


<import-file-form ref="ImportFile" @refresh="list"></import-file-form>
</a-card>
</template>

<script>
    import ImportFileForm from "@/components/common/ImportFileForm.vue";
    import {
        getPromiseList,
        deleTetePromise
    } from "@/api/serviceQuality/promise";

    export default {
        components: {
            ImportFileForm,
        },
        data() {
            return {
                isOpen: false,
                tablePageConfig: {
                    formItem: [{
                        component: "UnitSelect",
                        options: {
                            label: "填报单位",
                            prop: "operationUnitId",
                        },
                        attrs: {
                            ref: "UnitSelect",
                            isForm: false,
                            businessArae: "01",
                        },
                        on: {
                            unitChange: (operationUnitId) => {
                                if (!operationUnitId) {
                                    this.$refs.tablePage.queryParam.operationUnitId = "";
                                } else {
                                    this.$refs.tablePage.queryParam.operationUnitId =
                                        operationUnitId;
                                    this.list();
                                }
                            },
                        },
                    }, {
                        component: "YearPicker",
                        options: {
                            label: "年份",
                            prop: "promiseYear",
                        },
                        attrs: {
                            placeholder: "请选择年份",
                            style: "width: 220px",
                        },
                    }, ],
                    getAsyncDate: null,
                    columns: [{
                        title: "序号",
                        dataIndex: "index",
                        key: "id",
                        align: "center",
                        width: "80px",
                        scopedSlots: {
                            customRender: "index"
                        },
                    }, {
                        title: "填报单位",
                        dataIndex: "operationUnitName",
                        key: "operationUnitName",
                        align: "center",
                        width: "250px",
                    }, {
                        title: "承诺信息",
                        dataIndex: "detailStr",
                        key: "detailStr",
                        align: "center",
                        width: "700px",
                        scopedSlots: {
                            customRender: "detailStr"
                        },
                    }, {
                        title: "年份",
                        dataIndex: "promiseYear",
                        key: "promiseYear",
                        align: "center",
                    }, {
                        title: "更新时间",
                        align: "center",
                        dataIndex: "updateTime",
                        key: "updateTime",
                    }, {
                        title: "操作",
                        key: "operation",
                        align: "center",
                        width: "200px",
                        scopedSlots: {
                            customRender: "operation"
                        },
                    }, ],
                },
                copyQuery: {},
            };
        },
        created() {},
        mounted() {
            this.list();
        },

        methods: {
            //新增
            handleAdd() {
                this.$router.push({
                    path: "/serviceQuality/year/promise/modalForm",
                    query: {},
                });
            },

            //编辑
            handleUpDate(data) {
                this.$router.push({
                    path: "/serviceQuality/year/promise/modalForm",
                    query: {
                        id: data.id
                    },
                });
            },

            //删除
            handleDetails(data) {
                this.$router.push({
                    path: "/serviceQuality/year/promise/detials",
                    query: {
                        id: data.id,
                    },
                });
            },

            async handleDelete(id) {
                let {
                    message
                } = await deleTetePromise([id]);
                this.list();
            },
            list() {
                this.tablePageConfig.getAsyncDate = async(params, next) => {
                    // let query = { ...params, ...this.form };
                    let {
                        result
                    } = await getPromiseList(params);
                    this.copyQuery = {...params
                    };
                    next(result.records, result.total);
                };
            },
        },
    };
</script>
<style lang="less"></style>