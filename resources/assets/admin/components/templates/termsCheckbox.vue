<template>
    <div class="ff_tc">
        <el-checkbox v-if="item.settings.has_checkbox">
            <span v-html="item.settings.tnc_html"></span>
        </el-checkbox>
        <div v-else class="el-checkbox">
            <div class="el-checkbox__label" v-html="item.settings.tnc_html"></div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TNC_Checkbox',
    props: ['item']
}
</script>
<style>
    .ff_tc {
        overflow: hidden;
    }
    .ff_tc .el-checkbox__input {
        vertical-align: top;
    }
    .ff_tc .el-checkbox__label {
        overflow: hidden;
        word-break: break-word;
        white-space: break-spaces;
    }
</style>
