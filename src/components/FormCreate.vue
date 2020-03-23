<template>
  <section id="main" class="nc-main">
    <section class="el-container fm2-container is-vertical height-mp">
      <section class="el-container">
        <aside class="el-aside html-view">
          <div class="html-view-dev">
            <center>
              <h3>{{title}}</h3>
            </center>
            <div v-for="element in list2" :key="element.id" v-html="element.text"></div>
          </div>
        </aside>

        <section class="el-container center-container is-vertical">
          <header class="el-header btn-bar" style="height: 45px;">
            <button type="button" class="el-button el-button--text el-button--medium">
              <!---->
              <i class="el-icon-back"></i>
              <span>撤销</span>
            </button>
            <button
              disabled="disabled"
              type="button"
              class="el-button el-button--text el-button--medium is-disabled"
            >
              <!---->
              <i class="el-icon-right"></i>
              <span>重做</span>
            </button>
            <button type="button" class="el-button el-button--text el-button--medium">
              <!---->
              <i class="el-icon-upload2"></i>
              <span>导入JSON</span>
            </button>
            <button type="button" class="el-button el-button--text el-button--medium">
              <!---->
              <i class="el-icon-delete"></i>
              <span>清空</span>
            </button>
            <button type="button" class="el-button el-button--text el-button--medium">
              <!---->
              <i class="el-icon-tickets"></i>
              <span>生成JSON</span>
            </button>
            <button type="button" class="el-button el-button--text el-button--medium">
              <!---->
              <i class="el-icon-document"></i>
              <span>生成代码</span>
            </button>
          </header>
          <div class="el-main widget-empty">
            <div class="widget-form-container">
              <form class="el-form el-form--label-right">
                <div class="form-empty">从右侧拖拽或点击来添加字段</div>
                <div class="widget-form-list">
                  <center>
                    <span class="span-input-title">
                      <input class="balloon-title" type="text" v-model="title" placeholder="单行文本" />
                    </span>
                  </center>
                  <draggable :list="list2" group="people" class="dragArea list-group" @change="log">
                    <div class="list-group-item" v-for="(element,idx) in list2" :key="element.id">
                      <span v-if="element.type == 1" class="span-input">
                        <input class="balloon balloon-create" type="text" placeholder="单行文本" />
                        <el-row class="el-row-right">
                          <el-button
                            type="danger"
                            icon="el-icon-delete"
                            circle
                            @click="removeAt(idx)"
                          ></el-button>
                          <el-button
                            type="success"
                            class="el-button-create"
                            icon="el-icon-copy-document"
                            circle
                            @click="addSingle(element,idx)"
                          ></el-button>
                        </el-row>
                      </span>
                      <el-collapse
                        v-if="element.type == 2"
                        v-model="activeNames"
                        @change="handleChange"
                        style="height:35px;border-top: 0px;border-bottom: 0px;position: relative;"
                      >
                        <el-collapse-item title="单行文本" class="text-create" name="1">
                          <div class="text-design">
                            <el-form
                              :model="ruleForm"
                              ref="ruleForm"
                              label-width="100px"
                              class="demo-ruleForm"
                            >
                              <el-form-item label="活动名称" prop="name">
                                <el-input v-model="ruleForm.name"></el-input>
                              </el-form-item>
                              <el-form-item label="活动区域" prop="region">
                                <el-select v-model="ruleForm.region" placeholder="请选择活动区域">
                                  <el-option label="区域一" value="shanghai"></el-option>
                                  <el-option label="区域二" value="beijing"></el-option>
                                </el-select>
                              </el-form-item>
                              <el-form-item label="活动时间" required>
                                <el-col :span="11">
                                  <el-form-item prop="date1">
                                    <el-date-picker
                                      type="date"
                                      placeholder="选择日期"
                                      v-model="ruleForm.date1"
                                      style="width: 100%;"
                                    ></el-date-picker>
                                  </el-form-item>
                                </el-col>
                                <el-col class="line" :span="2">-</el-col>
                                <el-col :span="11">
                                  <el-form-item prop="date2">
                                    <el-time-picker
                                      placeholder="选择时间"
                                      v-model="ruleForm.date2"
                                      style="width: 100%;"
                                    ></el-time-picker>
                                  </el-form-item>
                                </el-col>
                              </el-form-item>
                              <el-form-item label="即时配送" prop="delivery">
                                <el-switch v-model="ruleForm.delivery"></el-switch>
                              </el-form-item>
                              <el-form-item label="活动性质" prop="type">
                                <el-checkbox-group v-model="ruleForm.type">
                                  <el-checkbox label="美食/餐厅线上活动" name="type"></el-checkbox>
                                  <el-checkbox label="地推活动" name="type"></el-checkbox>
                                  <el-checkbox label="线下主题活动" name="type"></el-checkbox>
                                  <el-checkbox label="单纯品牌曝光" name="type"></el-checkbox>
                                </el-checkbox-group>
                              </el-form-item>
                              <el-form-item label="特殊资源" prop="resource">
                                <el-radio-group v-model="ruleForm.resource">
                                  <el-radio label="线上品牌商赞助"></el-radio>
                                  <el-radio label="线下场地免费"></el-radio>
                                </el-radio-group>
                              </el-form-item>
                              <el-form-item label="活动形式" prop="desc">
                                <el-input type="textarea" v-model="ruleForm.desc"></el-input>
                              </el-form-item>
                              <el-form-item>
                                <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
                                <el-button @click="resetForm('ruleForm')">重置</el-button>
                              </el-form-item>
                            </el-form>
                          </div>
                        </el-collapse-item>
                        <el-row class="el-row-right">
                          <el-button
                            type="danger"
                            icon="el-icon-delete"
                            circle
                            @click="removeAt(idx)"
                          ></el-button>
                          <el-button
                            type="success"
                            class="el-button-create"
                            icon="el-icon-copy-document"
                            circle
                            @click="addSingle(element,idx)"
                          ></el-button>
                        </el-row>
                      </el-collapse>
                    </div>
                  </draggable>
                </div>
              </form>
            </div>
          </div>
        </section>
        <aside class="el-aside html-create">
          <div class="components-list">
            <draggable
              class="dragArea list-group"
              tag="ul"
              v-model="list"
              :group="{ name: 'people', pull: 'clone', put: false }"
              :clone="cloneDog"
            >
              <li
                class="form-edit-widget-label"
                v-for="element in list"
                :key="element.id"
                @click="addSingleText"
              >
                <a>
                  <i class="el-icon-postcard"></i>
                  <span>{{element.name}}</span>
                </a>
              </li>
            </draggable>
          </div>
        </aside>
      </section>
    </section>
  </section>
</template>
<script>
import draggable from "vuedraggable";
const message = [
  {
    id: 1,
    name: "单行文本",
    type: 1,
    text:
      '<span class="span-input"><input class="balloon" type="text" placeholder="单行文本" /><label>单行文本</label></span>'
  },
  {
    id: 2,
    name: "多行文本",
    type: 2
  }
];
let idGlobal = 2;
let html = "";
export default {
  name: "hello",
  components: {
    draggable
  },
  data() {
    return {
      title: "标题",
      list: message,
      list2: [],
      ruleForm: {
        name: "",
        region: "",
        date1: "",
        date2: "",
        delivery: false,
        type: [],
        resource: "",
        desc: ""
      },
      rules: {
        name: [
          { required: true, message: "请输入活动名称", trigger: "blur" },
          { min: 3, max: 5, message: "长度在 3 到 5 个字符", trigger: "blur" }
        ],
        region: [
          { required: true, message: "请选择活动区域", trigger: "change" }
        ],
        date1: [
          {
            type: "date",
            required: true,
            message: "请选择日期",
            trigger: "change"
          }
        ],
        date2: [
          {
            type: "date",
            required: true,
            message: "请选择时间",
            trigger: "change"
          }
        ],
        type: [
          {
            type: "array",
            required: true,
            message: "请至少选择一个活动性质",
            trigger: "change"
          }
        ],
        resource: [
          { required: true, message: "请选择活动资源", trigger: "change" }
        ],
        desc: [{ required: true, message: "请填写活动形式", trigger: "blur" }]
      }
    };
  },
  methods: {
    log: function(evt) {
      window.console.log(evt);
    },
    handleChange(val) {
      console.log(val);
    },
    removeAt(idx) {
      this.list2.splice(idx, 1);
    },
    cloneDog({ id, name, type }) {
      return {
        id: idGlobal++,
        name: name,
        type: type,
        text:
          '<span class="span-input"><input class="balloon" type="text" placeholder="单行文本" /><label>单行文本</label></span>'
      };
    },
    addSingleText: function() {
      var arr = {
        id: idGlobal++,
        name: "单行文本",
        type: 1,
        text:
          '<span class="span-input"><input class="balloon" type="text" placeholder="单行文本" /><label>单行文本</label></span>'
      };
      this.list2.push(arr);
    },
    addSingle: function(element, idx) {
      var arr = {
        id: idGlobal++,
        name: element.name,
        type: element.type,
        text:
          '<span class="span-input"><input class="balloon" type="text" placeholder="单行文本" /><label>单行文本</label></span>'
      };
      this.list2.splice(idx + 1, 0, arr);
    }
  },
  computed: {
    dragOptions() {
      return {
        animation: 0,
        group: "description",
        disabled: !this.editable,
        ghostClass: "ghost"
      };
    },
    listString() {
      return JSON.stringify(this.list, null, 2);
    },
    list2String() {
      return JSON.stringify(this.list2, null, 2);
    }
  }
};
</script>
<style>
#main {
  height: 100%;
  margin: 0;
  padding: 0;
  line-height: 1.15;
}
.center-container .btn-bar {
  height: 45px;
  line-height: 45px;
  font-size: 18px;
  text-align: center;
}
.height-mp {
  height: 100%;
}
.widget-form-container {
  height: 100%;
}
.center-container .el-main {
  padding: 0;
  position: relative;
  background: #fafafa;
  height: calc(95% - 10px);
}
.widget-form-container .widget-form-list {
  background: #fff;
  border: 1px dashed #999;
  min-height: 100%;
  padding: 10px;
}
.widget-form-container form,
.widget-form-container form > div {
  height: calc(100% - 10px);
}
.widget-form-container .form-empty {
  position: absolute;
  text-align: center;
  width: 300px;
  height: 20px;
  font-size: 20px;
  top: 50%;
  width: 100%;
  margin-top: -10px;
  color: #ccc;
}
.html-create {
  width: 20%;
}
.html-view {
  width: 40%;
}
.components-list {
  padding: 8px 0;
  width: 100%;
  height: 100%;
}
.components-list ul {
  position: relative;
  overflow: hidden;
  padding: 0 10px 0px;
  margin-top: 35px;
}
.form-edit-widget-label {
  font-size: 12px;
  display: block;
  width: 40%;
  line-height: 26px;
  position: relative;
  float: left;
  left: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  margin: 1%;
  color: #333;
  border: 1px solid #f4f6fc;
}
.components-list .form-edit-widget-label > a {
  display: block;
  cursor: move;
  background: #f4f6fc;
  border: 1px solid #f4f6fc;
}
.components-list .form-edit-widget-label > a span {
  display: inline-block;
  vertical-align: middle;
}
.components-list .form-edit-widget-label > a > i {
  margin-right: 6px;
  margin-left: 8px;
  font-size: 14px;
  display: inline-block;
  vertical-align: middle;
}
.html-view-dev {
  background: #fff;
  border: 1px dashed #999;
  height: 93.89%;
  margin-top: 45px;
  margin-right: 5px;
  margin-left: 5px;
  padding: 10px;
}
.components-list .form-edit-widget-label:hover a {
  color: #409eff;
}
.components-list .form-edit-widget-label:hover {
  color: #409eff;
  border: 1px dashed #409eff;
}
.flip-list-move {
  transition: transform 0.5s;
}
.no-move {
  transition: transform 0s;
}
.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}
.list-group {
  min-height: 20px;
}
.list-group-item {
  cursor: move;
  display: inline-block;
  padding: 2px;
  width: 100%;
}
.list-group-item i {
  cursor: pointer;
}
.span-input {
  position: relative;
  display: inline-block;
  width: 100%;
}
.span-input input:focus {
  border: 1px solid #fafafa;
  -webkit-box-shadow: 0px 0px 6px #007eff;
  -moz-box-shadow: 0px 0px 5px #007eff;
  box-shadow: 0px 0px 5px #007eff;
}
.balloon {
  display: inline-block;
  width: 95%;
  padding: 10px 0 10px 15px;
  font-family: "Open Sans", sans;
  font-weight: 400;
  color: #377d6a;
  border: 1px solid #ccc;
  border-radius: 3px;
  outline: 0;
  text-indent: 60px;
  transition: all 0.3s ease-in-out;
}
.balloon-create {
  text-indent: 0px;
}
.balloon::-webkit-input-placeholder {
  color: #fff;
  text-indent: 0;
  font-weight: 300;
}
.balloon-create::-webkit-input-placeholder {
  color: #032429;
  text-indent: 0;
  font-weight: 300;
}
.balloon + label {
  display: inline-block;
  position: absolute;
  top: 8px;
  left: 0;
  bottom: 8px;
  padding: 5px 15px;
  color: #032429;
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
  text-shadow: 0 1px 0 rgba(19, 74, 70, 0);
  transition: all 0.3s ease-in-out;
  border-radius: 3px;
  background: rgba(122, 184, 147, 0);
}
.balloon + label:after {
  position: absolute;
  content: "";
  width: 0;
  height: 0;
  top: 100%;
  left: 50%;
  margin-left: -3px;
  border-left: 3px solid transparent;
  border-right: 3px solid transparent;
  border-top: 3px solid rgba(122, 184, 147, 0);
  transition: all 0.3s ease-in-out;
}

.balloon:focus,
.balloon:active {
  color: #377d6a;
  text-indent: 0;
  background: #fff;
}
.balloon:focus::-webkit-input-placeholder,
.balloon:active::-webkit-input-placeholder {
  color: #aaa;
}
.balloon:focus + label,
.balloon:active + label {
  color: #fff;
  text-shadow: 0 1px 0 rgba(19, 74, 70, 0.4);
  background: #7ab893;
  transform: translateY(-40px);
}
.balloon:focus + label:after,
.balloon:active + label:after {
  border-top: 4px solid #7ab893;
}
.span-input-title {
  position: relative;
  display: inline-block;
  width: 70%;
  margin-bottom: 20px;
}
.span-input-title input:focus {
  border: 1px solid #fafafa;
  -webkit-box-shadow: 0px 0px 6px #007eff;
  -moz-box-shadow: 0px 0px 5px #007eff;
  box-shadow: 0px 0px 5px #007eff;
}
.balloon-title {
  display: inline-block;
  width: 95%;
  padding: 10px 0 10px 15px;
  font-family: "Open Sans", sans;
  font-weight: 400;
  color: #377d6a;
  border: 1px solid #ccc;
  border-radius: 3px;
  outline: 0;
  transition: all 0.3s ease-in-out;
  text-align: center;
}
.el-row-right {
  text-align: right;
}
.el-button-create {
  margin-left: 0px !important;
}

.balloon + div {
  display: inline-block;
  position: absolute;
  top: 0px;
  right: 0;
  bottom: 8px;
  color: #032429;
  font-size: 11px;
  font-weight: 700;
  opacity: 1;
}
.text-create + div {
  display: inline-block;
  position: absolute;
  top: 0px;
  right: 0;
  bottom: 8px;
  color: #032429;
  font-size: 11px;
  font-weight: 700;
  opacity: 1;
}
.text-create {
  display: inline-block;
  width: 100%;
  font-family: "Open Sans", sans;
  font-weight: 400;
  color: #377d6a;
  border: 1px solid #ccc;
  border-radius: 3px;
  outline: 0;
  -webkit-transition: all 0.3s ease-in-out;
  transition: all 0.3s ease-in-out;
}
.text-create .el-collapse-item__header {
  height: 35px;
  line-height: 35px;
  font-size: 12px;
  padding: 0px 0 0px 15px;
  border-bottom: 0px;
}
.text-create .el-collapse-item__content {
  font-size: 12px;
}
.text-create .el-collapse-item__arrow {
  display: none;
}
.text-create .text-design {
  padding: 0px 5px 0px 5px;
}
.text-create .el-collapse-item__content {
  padding-bottom: 0px;
}
.list-group {
  min-height: 100%;
}
.el-button.is-circle {
  padding: 4px;
}
</style>