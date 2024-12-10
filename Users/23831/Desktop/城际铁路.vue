<template>
    <a-form-model ref="ruleForm" :model="form" :rules="rules" class="layui-form-item " :layout="formLayout"
        style="background-color: white;" :form="form" :label-col="{ span: 4 }" :wrapper-col="{ span: 5 }"
        value="left">
        <a-row type="flex" justify="space-around">
            <a-form-model-item value="left" ref="region" :span="8" prop="region" label="运营单位" :labelCol="labelCol" :wrapperCol="wrapperCol">
                <a-select placeholder="广东城际铁路运营有限公司" style="width: 300px;" v-model="form.region" allowClear @clear="aaa">
                    <a-select-option value="广东城际铁路运营有限公司" >
                        广东城际铁路运营有限公司
                    </a-select-option>
                    <a-select-option value="中国铁路广州局集团有限公司">
                        中国铁路广州局集团有限公司
                    </a-select-option>
                </a-select>
            </a-form-model-item>


            <a-form-model-item  ref="regions" :span="8" prop="regions" label="业务领域" :labelCol="labelCol" :wrapperCol="wrapperCol">
                <a-select placeholder="城际铁路" style="width: 300px;"  v-model="regions" allowClear @clear="aaa"  disabled="true">
                    <a-select-option value="城际铁路">
                        城际铁路
                    </a-select-option>
                </a-select>
            </a-form-model-item>

            <a-form-model-item ref="name" :span="8" prop="name" label="预案名称" :labelCol="labelCol" :wrapperCol="wrapperCol">
                <a-input placeholder="请输入" v-model="form.name"  style="width: 300px;"
                    @blur="() => { $refs.name.onFieldBlur() }" />
            </a-form-model-item>
        </a-row>

        <a-row type="flex" justify="space-around">
            <a-form-model-item ref="number" :span="8" prop="number" label="预案文号" :labelCol="labelCol" :wrapperCol="wrapperCol">
                <a-input placeholder="请输入" style="width: 300px;" @blur="() => { $refs.number.onFieldBlur() }"
                    v-model="form.number" />
            </a-form-model-item>
            <a-form-model-item :span="8" prop="regions" ref="regions" label="预案类型" :labelCol="labelCol" :wrapperCol="wrapperCol">
                <a-select placeholder="请选择" @blur="() => { $refs.regions.onFieldBlur() }" style="width: 300px;" allowClear @clear="aaa"
                    v-model="form.regions">
                    <a-select-option value="综合应急预案">
                        综合应急预案
                    </a-select-option>
                    <a-select-option value="专项应急预案">
                        专项应急预案
                    </a-select-option>
                    <a-select-option value="现场处置方案">
                        现场处置方案
                    </a-select-option>
                </a-select>
            </a-form-model-item>
            <a-form-model-item :span="8" required prop="date2" ref="date2" label="发布日期" :labelCol="labelCol" :wrapperCol="wrapperCol">
                <a-date-picker placeholder="请选择" @blur="() => { $refs.date2.onFieldBlur() }" v-model="form.date2" show-time
                    @datePicker="onDatePicker" style="width: 300px;"  />
            </a-form-model-item>
        </a-row>



        <a-row type="flex" justify="space-around" style="margin-right: 510px;">
            <a-form-model-item :span="6" required prop="date1" ref="date1" label="实施日期" :labelCol="labelCol" :wrapperCol="wrapperCol">
                <a-date-picker @blur="() => { $refs.date1.onFieldBlur() }" show-time @datePicker="onDatePicker"
                    style="width: 300px;" placeholder="请选择" v-model="form.date1" />
            </a-form-model-item>
            <a-form-model-item :span="6" prop="bianzhi" ref="bianzhi" label="编制部门" :labelCol="labelCol" :wrapperCol="wrapperCol">
                <a-select placeholder="请选择" style="width: 300px;" v-model="form.bianzhi" allowClear @clear="aaa">
                    <a-select-option value="运营部" >
                        运营部
                    </a-select-option>
                </a-select>
            </a-form-model-item>
        </a-row>
        <a-row type="flex" justify="space-around" style="margin-right: 510px;">
            <a-form-model-item ref="regioned" :span="6" prop="regioned" label="应急演练频次" :labelCol="labelCol" :wrapperCol="wrapperCol">
                <div style="display:inline-block; width: 300px;">

                    <a-select slot="addonBefore" placeholder="请选择" allowClear @clear="aaa" style="width: 150px;" v-model="form.regioned">
                        <a-select-option value="每天" >每天</a-select-option>
                        <a-select-option value="每周" >每周</a-select-option>
                        <a-select-option value="每月" >每月</a-select-option>
                        <a-select-option value="每季" >每季</a-select-option>
                        <a-select-option value="每年" >每年</a-select-option>
                    </a-select>
                    <a-input addon-after="次" style="width: 120px;text-align: center;" ref="regioned" ></a-input>

                </div>
            </a-form-model-item>
            <a-form-model-item :span="6" prop="fanwei" ref="fanwei" label="适用范围" :labelCol="labelCol" :wrapperCol="wrapperCol">
                <a-input placeholder="请输入" style="width: 300px;" v-model="form.fanwei" />
            </a-form-model-item>
        </a-row>




        <a-row type="flex" justify="space-around" style="margin-right: 370px;">
            <a-from-model-item
            ref="avatar"
            :span="6"
            prop="avatar"
            style="margin-right: 160px;"
            >应急处置流程
                <a-upload
                    list-type="picture-card"
                    class="avatar-uploader gin"
                    :show-upload-list="false"
                    action=""
                    :before-upload="beforeUpload"
                    :span="6"
                    v-model="form.avatar">
                    <img v-if="imageUrl" :src="imageUrl" alt="avatar" />
                    <div v-else>
                        <a-icon :type="loading ? 'loading' : 'plus'" />
                        <div class="ant-upload-text">
                        </div>
                    </div>
                </a-upload>
            </a-from-model-item>
            <a-form-model-item ref="fujian" :span="6" prop="fujian" label="应急预案附件" :labelCol="labelCol" :wrapperCol="wrapperCol">
                <a-button style="width: 120px;flex: left;" :span="6" name="file" :multiple="true" action="" :headers="headers" v-model="form.fujian">
                        <a-icon type="upload" />上传文件
                    </a-button>
                <p class="moren" style="width: 400px;">支持rar/zip/doc/docx文件,不超过100M</p>
            </a-form-model-item>
        </a-row>
    </a-form-model>







</template>
<script>
import { message } from 'ant-design-vue';
import { compact } from 'lodash-es';
import { before } from 'lodash-es';


function aaa(){
    console.log('清除触发了');
    
}
export default {
    method: {
        //日期
        onDatePicker(date, dateString) {
            console.log(date, dateString);
        },
        //上传
        beforeUpload(file) {
            const isJpgOrPng = file.type === 'image/jpeg' || file.type === 'image/png';
            if (!isJpgOrPng) {
                this.$message.error('You can only upload JPG file!');
            }
            const isLt2M = file.size / 1024 / 1024 < 2;
            if (!isLt2M) {
                this.$message.error('Image must smaller than 2MB!');
            }
            return isJpgOrPng && isLt2M;
        },

    },

    data() {
        return {
            //input、文本框与label换行
            labelCol: {
                xs: { span: 5 },
                sm: { span: 5 },
            },
            wrapperCol: {
                xs: { span: 20 },
                sm: { span: 20 },
            },

            loading: false,
            imgUrl: '',
            headers: {
                authorization: 'authorization-text',
            },

            form: {
                layout: 'horizontal',
                region: '',
                regions: '',
                name: '',
                number: '',
                date1: '',
                date2: '',
                bianzhi: '',
                fanwei: '',
                avatar: '',
                fujian: '',
            },

            rules: {
                region: [{ required: true, message: '不能为空', trigger: 'blur' }],
                name: [{ required: true, message: '不能为空', trigger: 'blur' }],
                regions: [{ required: true, message: '不能为空', trigger: 'blur' }],
                number: [{ required: true, message: '不能为空', trigger: 'blur' }],
                date1: [{ required: true, message: '不能为空', trigger: 'blur' }],
                date2: [{ required: true, message: '不能为空', trigger: 'blur' }],
                bianzhi: [{}],
                fanwei: [{}],
                avatar: [{}],
                fujian: [{ required: true, message: '不能为空', trigger: 'blur' }],
            },
        }


},

}
</script>
<style>
.avatar-uploader>.ant-upload {
    width: 128px;
    height: 128px;
}

.ant-upload-select-picture-card i {
    font-size: 32px;
    color: #999;
}

.ant-upload-select-picture-card .ant-upload-text {
    margin-top: 8px;
    color: #666;
}

.moren {
    color: #7e7e7e;
    font-size: 10px;

}

.a-form-model-item__label {
    float: none;
    word-break: break-word;
}
</style>