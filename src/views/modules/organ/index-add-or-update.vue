<template>
  <el-dialog
    :visible.sync="visible"
    :title="!dataForm.id ? $t('add') : $t('update')"
    :close-on-click-modal="false"
    :close-on-press-escape="false"
  >
    <el-form
      :model="dataForm"
      :rules="dataRule"
      ref="dataForm"
      @keyup.enter.native="dataFormSubmitHandle()"
      label-width="120px"
    >
      <el-form-item prop="orgName" :label="'机构' + $t('dept.name')">
        <el-input
          v-model="dataForm.orgName"
          :placeholder="'机构' + $t('dept.name')"
          maxlength="30"
          show-word-limit
        ></el-input>
      </el-form-item>
      <el-form-item prop="telephone" label="电话">
        <el-input
          v-model="dataForm.telephone"
          :placeholder="电话"
          maxlength="30"
          show-word-limit
        ></el-input>
      </el-form-item>
      <el-form-item prop="cityId" label="所属地域">
        <el-cascader
          style="width:100%;"
          v-model="dataForm.cityId"
          placeholder="选择区域"
          :options="regionTree"
          :props="optionsProps"
          @change="handleChange"
          clearable
        ></el-cascader>
      </el-form-item>
    </el-form>
    <template slot="footer">
      <el-button @click="visible = false">{{ $t("cancel") }}</el-button>
      <el-button type="primary" @click="dataFormSubmitHandle()">{{
        $t("confirm")
      }}</el-button>
    </template>
  </el-dialog>
</template>

<script>
import debounce from "lodash/debounce";
import { treeDataTranslate } from "@/utils";
export default {
  props: {
    regionTree: {
      type: Array,
      default: () => [],
    },
    optionsProps: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      visible: false,
      dataList: [],
      dataForm: {
        id: "",
        orgName: "",
        telephone: "",
        cityId: null,
      },
    };
  },
  computed: {
    dataRule() {
      return {
        orgName: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "blur",
          },
        ],
        cityId: [
          {
            required: true,
            message: this.$t("validate.required"),
            trigger: "change",
          },
        ],
      };
    },
  },
  mounted() {
    // this.getDataList();
  },
  methods: {
    init() {
      this.visible = true;
      this.$nextTick(() => {
        this.$refs["dataForm"].resetFields();
        if (this.dataForm.id) {
          this.getInfo();
        }
      });
    },

    handleChange(value) {
      this.dataForm.cityId = value[value.length - 1];
    },
    // 获取信息
    getInfo() {
      this.$http
        .get(`/sys/org/getOrgDetail`, { params: { id: this.dataForm.id } })
        .then(({ data: res }) => {
          if (res.code !== 0) {
            return this.$message.error(res.msg);
          }
          this.dataForm = {
            ...this.dataForm,
            ...res.data,
          };
        })
        .catch(() => {});
    },
    getDataList() {
      return this.$http
        .get("/sys/region/tree")
        .then(({ data: res }) => {
          if (res.code !== 0) {
            return this.$message.error(res.msg);
          }
          this.dataList = treeDataTranslate(res.data);
          console.log(this.dataList);
        })
        .catch(() => {});
    },
    // 表单提交
    dataFormSubmitHandle: debounce(
      function() {
        this.$refs["dataForm"].validate((valid) => {
          if (!valid) {
            return false;
          }
          this.$http["post"](
            !this.dataForm.id ? "/sys/org/add" : "/sys/org/updateOrg",
            this.dataForm
          )
            .then(({ data: res }) => {
              if (res.code !== 0) {
                return this.$message.error(res.msg);
              }
              this.$message({
                message: this.$t("prompt.success"),
                type: "success",
                duration: 500,
                onClose: () => {
                  this.visible = false;
                  this.$emit("refreshDataList");
                },
              });
            })
            .catch(() => {});
        });
      },
      1000,
      { leading: true, trailing: false }
    ),
  },
};
</script>

<style lang="scss">
.mod-sys__dept {
  .dept-list {
    .el-input__inner,
    .el-input__suffix {
      cursor: pointer;
    }
  }
}
</style>
