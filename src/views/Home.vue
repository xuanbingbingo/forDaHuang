<template>
  <div class="home">
    <div class="commonSearchContent">
      <div class="searchCommonNew">
        <el-form :inline="true" :label-width="formLabelWidth">
          <el-form-item label="请选择系统">
            <el-select
              size="small"
              class="el-custom-input-width200"
              v-model="systemTypeName"
              filterable
              clearable
              placeholder="全部"
              @change="changeType"
            >
              <el-option
                v-for="(val,index) in systemList"
                :key="val.name"
                :label="val.name"
                :value="val.rule"
              ></el-option>
            </el-select>
          </el-form-item>
        </el-form>
      </div>
      <div class="searchCommonNew">
        <el-form :inline="true" :label-width="formLabelWidth">
          <el-form-item label="索引">
            <el-input
              v-model="index"
              size="small"
              class="el-custom-input-width200"
              :clearable="true"
              placeholder="请输入索引"
            />
          </el-form-item>
        </el-form>
      </div>
      <div class="searchCommonNew">
        <el-form :inline="true" :label-width="formLabelWidth">
          <el-form-item label="索引类型">
            <el-input
              v-model="index_type"
              size="small"
              class="el-custom-input-width200"
              :clearable="true"
              placeholder="请输入索引类型"
            />
          </el-form-item>
          <el-form-item label="索引类型">
            <el-input
              v-model="rack"
              size="small"
              class="el-custom-input-width200"
              :clearable="true"
              placeholder="请输入机架号"
            />
          </el-form-item>
          <el-form-item label="frame">
            <el-input
              v-model="frame"
              size="small"
              class="el-custom-input-width200"
              :clearable="true"
              placeholder="请输入机框号"
            />
          </el-form-item>
        </el-form>
      </div>
      <div class="searchCommonNew">
        <el-form :inline="true" :label-width="formLabelWidth">
          <el-form-item label="槽位号">
            <el-input
              v-model="slot"
              size="small"
              class="el-custom-input-width200"
              :clearable="true"
              placeholder="请输入槽位号"
            />
          </el-form-item>
          <el-form-item label="PON口号">
            <el-input
              v-model="pon"
              size="small"
              class="el-custom-input-width200"
              :clearable="true"
              placeholder="请输入PON口号"
            />
          </el-form-item>
          <el-form-item label="ONU号">
            <el-input
              v-model="onu"
              size="small"
              class="el-custom-input-width200"
              :clearable="true"
              placeholder="请输入ONU号"
            />
          </el-form-item>
          <el-form-item label="ONU端口号">
            <el-input
              v-model="port"
              size="small"
              class="el-custom-input-width200"
              :clearable="true"
              placeholder="请输入ONU端口号"
            />
          </el-form-item>
          <el-form-item label="PVC端口号">
            <el-input
              v-model="pvc"
              size="small"
              class="el-custom-input-width200"
              :clearable="true"
              placeholder="PVC端口号"
            />
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

interface System {
  name: string
  rule: string
}

@Component
export default class HelloWorld extends Vue {
  @Prop() private msg!: string;
  private systemList:Array<System> = [
    {"name":"AN5516","rule":"1"},
    {"name":"AN6001-G16","rule":"2"},
    {"name":"AN6000-2","rule":"3"},
    {"name":"AN6000-17","rule":"4"}
  ]
  private formLabelWidth :string= "100px";
  private systemTypeName :string = "";
  private currentRule: string = "";
  private index: string = "0";
  private index_type: string = "0";
  private rack: string = "0";
  private frame: string = "0";
  private slot: string = "0";
  private pon: string = "0";
  private onu: string = "0";
  private port: string = "0";
  private pvc: string = "0";
  private changeType(val:string){
    this.currentRule = val;
  }
}
</script>

<style scoped lang="scss">
.home{
  padding: 30px;
  .commonSearchContent{
    .searchCommonNew{
      margin-top:30px;
    }
  }
}
</style>
