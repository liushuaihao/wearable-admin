<template>
  <div>
    <p>
      <el-row :gutter="10">
        <el-col :span="4">姓名：{{ basicInformation.name }}</el-col>
        <el-col :span="4">性别：{{ basicInformation.sex }}</el-col>
        <el-col :span="4">年龄：{{ basicInformation.age }}</el-col>
        <el-col :span="4">身高：{{ basicInformation.stature }}cm</el-col>
        <el-col :span="4">体重：{{ basicInformation.weight }}kg</el-col>
      </el-row>
    </p>
    <p>电话：{{ basicInformation.tel }}</p>
    <p>住址：{{ basicInformation.site }}</p>
    <p>紧急联系人</p>
    <p>张儿子：16601275207</p>
    <p>张女儿：16601275207</p>
    <p>张弟弟：16601275207</p>
    <p>关联设备</p>
    <p>主控设备ID：1111-0000-1111</p>
    <p>主控设备通讯号：16601275207</p>
  </div>
</template>
<script>
export default {
  props: {
    userId: {
      type: [Number, String],
      default: () => "",
    },
  },
  data() {
    return {
      dataForm: {
        emergencyList: [],
      },
      basicInformation: {
        name: "张三",
        sex: "男",
        age: "22",
        stature: "165",
        weight: "70",
        tel: "16601275207",
        site: "北京市海淀区中关村科技大厦502",
      },
    };
  },
  methods: {
    info() {
      this.$http
        .get("/device/devicebin/getPatientDetailAndDevice", {
          params: {
            userId: this.userId,
          },
        })
        .then((res) => {
          console.log(res);
        });
    },
  },
};
</script>
