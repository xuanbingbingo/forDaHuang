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
                :value="val.name"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="选择转换方向">
            <el-switch
              v-model="switchType"
              style="margin-top:-2px"
              active-text="索引转对象"
              inactive-text="对象转索引">
            </el-switch>
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
              :disabled="disableRule.index_type"
              placeholder="请输入索引类型"
            />
          </el-form-item>
          <el-form-item label="机架号">
            <el-input
              v-model="rack"
              size="small"
              class="el-custom-input-width200"
              :clearable="true"
              :disabled="disableRule.rack"
              placeholder="请输入机架号"
            />
          </el-form-item>
          <el-form-item label="机框号">
            <el-input
              v-model="frame"
              size="small"
              class="el-custom-input-width200"
              :clearable="true"
              :disabled="disableRule.frame"
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
              :disabled="disableRule.slot"
              placeholder="请输入槽位号"
            />
          </el-form-item>
          <el-form-item label="PON口号">
            <el-input
              v-model="pon"
              size="small"
              class="el-custom-input-width200"
              :clearable="true"
              :disabled="disableRule.pon"
              placeholder="请输入PON口号"
            />
          </el-form-item>
          <el-form-item label="ONU号">
            <el-input
              v-model="onu"
              size="small"
              class="el-custom-input-width200"
              :clearable="true"
              :disabled="disableRule.onu"
              placeholder="请输入ONU号"
            />
          </el-form-item>
          <el-form-item label="ONU端口号">
            <el-input
              v-model="port"
              size="small"
              class="el-custom-input-width200"
              :clearable="true"
              :disabled="disableRule.port"
              placeholder="请输入ONU端口号"
            />
          </el-form-item>
          <el-form-item label="PVC端口号">
            <el-input
              v-model="pvc"
              size="small"
              class="el-custom-input-width200"
              :clearable="true"
              :disabled="disableRule.pvc"
              placeholder="PVC端口号"
            />
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
interface Num{
  numberOne: number,
  numberTwo: number
}
interface Rule{
  slot?: Num,
  pon?: Num,
  onu?: Num,
  port?: Num
}
interface System {
  name: string
  rule: Rule
}
@Component
export default class HelloWorld extends Vue {
  private switchType:boolean = true; //默认是索引转对象
  private systemList:Array<System> = [
    {"name":"AN5516","rule":{
      "slot": {"numberOne":25,"numberTwo":0x3f},
      "pon": {"numberOne":19,"numberTwo":0x3f},
      "onu": {"numberOne":8,"numberTwo":0x7ff},
      "port": {"numberOne":0,"numberTwo":0xff},
    }},
    {"name":"AN6001-G16","rule":{}},
    {"name":"AN6000-2","rule":{}},
    {"name":"AN6000-17","rule":{}}
  ]
  private formLabelWidth :string= "100px";
  private systemTypeName :string = "AN5516";
  private currentRule!: Rule | any;

  private index: string = "0";

  private index_type: string = "0";
  private rack: string = "0";
  private frame: string = "0";

  private slot: string = "0";
  private pon: string = "0";
  private onu: string = "0";
  private port: string = "0";
  private pvc: string = "0";

  private disableRule:any = {};

  @Watch("index")
  private indexChange(val:string){
    if(this.switchType){
      this.computeObj(val);
    }
  }
  @Watch("slot")
  private slotChange(val:string){
    this.computeIndex();
  }
  @Watch("pon")
  private ponChange(val:string){
    this.computeIndex();
  }
  @Watch("onu")
  private onuChange(val:string){
    this.computeIndex();
  }
  @Watch("port")
  private portChange(val:string){
    this.computeIndex();
  }
  @Watch("switchType")
  private switchTypeChange(val:boolean){
    if(val){
      this.computeObj(this.index);
    }else{
      this.computeIndex();
    }
  }

  // 自定义禁用规则
  private getDisableRule(){
    switch(this.systemTypeName){
      case "AN5516":
        // true代表禁用
        this.disableRule = {
          index_type: true,
          rack:true,
          frame: true,
          slot: false,
          pon: false,
          onu: false,
          port: false,
          pvc: true
        }
        break;
      default:
        // 默认全都不禁用
        this.disableRule = {
          index_type: false,
          rack:false,
          frame: false,
          slot: false,
          pon: false,
          onu: false,
          port: false,
          pvc: false
        }
    }
  }
  // 根据索引计算对象
  private computeObj(val:string){
    let index = Number.parseInt(val);
    // this.slot = ((index >> 25) & 0x3f).toString();
    // this.pon = ((index >> 19) & 0x3f).toString();
    // this.onu = ((index >> 8) & 0x7ff).toString();
    // this.port = ((index) & 0xff).toString();
    const arrTemp:string[] = ["slot","pon","onu","port"];
    const currentRule = this.currentRule;
    let _this:any = this;
    arrTemp.forEach((item:string) => {
      _this[item] = ((index >> currentRule[item].numberOne) & currentRule[item].numberTwo).toString();
    })
  }
  // 根据对象计算索引
  private computeIndex(){
    const currentRule = this.currentRule;
    if(!this.switchType){
      this.index = (
        parseInt(this.slot)*Math.pow(2,currentRule.slot.numberOne) + 
        parseInt(this.pon)*Math.pow(2,currentRule.pon.numberOne) + 
        parseInt(this.onu)*Math.pow(2,currentRule.onu.numberOne) + 
        parseInt(this.port)*Math.pow(2,currentRule.port.numberOne)
      ).toString();
    }
  }
  // 切换系统(同时切换规则)
  private changeType(systemTypeName:string){
    const currentRule:any = this.systemList.find((item:System) => item.name === systemTypeName);
    if(currentRule){
      this.currentRule = currentRule.rule;
    }
    // 同时切换禁用规则
    this.getDisableRule();
  }
  private created(){
    // 初始化(未选择系统时)默认第一个系统的规则为当前规则
    this.currentRule = this.systemList[0].rule;
    this.getDisableRule();
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
