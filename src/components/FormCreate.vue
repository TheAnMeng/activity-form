<template>
  <section id="main" class="nc-main">
    <section class="el-container fm2-container is-vertical height-mp">
      <section class="el-container">
        <aside class="el-aside html-view">
          <div class="html-view-dev">
            <center>
              <h3>{{title}}</h3>
            </center>
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
                    <div
                      class="list-group-item"
                      v-for="element in list2"
                      :key="element.id"
                      v-html="element.text"
                    ></div>
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
            >
              <li
                class="form-edit-widget-label"
                v-for="element in list"
                :key="element.id"
                @click="add"
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
    text:
      "<span class='span-input'><input class='balloon' type='text' placeholder='单行文本' /></span>",
    type: 1
  },
  {
    id: 2,
    name: "多行文本",
    text:
      '<div role="tablist" aria-multiselectable="true" class="el-collapse"><div class="el-collapse-item"><div role="tab" aria-controls="el-collapse-content-4938" aria-describedby="el-collapse-content-4938"><div role="button" id="el-collapse-head-4938" tabindex="0" class="el-collapse-item__header">一致性 Consistency<i class="el-collapse-item__arrow el-icon-arrow-right"></i></div></div><div role="tabpanel" aria-labelledby="el-collapse-head-4938" id="el-collapse-content-4938" class="el-collapse-item__wrap" data-old-padding-top="" data-old-padding-bottom="" data-old-overflow="" style="display: none;" aria-hidden="true"><div class="el-collapse-item__content"><div>与现实生活一致：与现实生活的流程、逻辑保持一致，遵循用户习惯的语言和概念；</div> <div>在界面中一致：所有的元素和结构需保持一致，比如：设计样式、图标和文本、元素的位置等。</div></div></div></div> <div class="el-collapse-item"><div role="tab" aria-controls="el-collapse-content-5358" aria-describedby="el-collapse-content-5358"><div role="button" id="el-collapse-head-5358" tabindex="0" class="el-collapse-item__header">反馈 Feedback<i class="el-collapse-item__arrow el-icon-arrow-right"></i></div></div><div role="tabpanel" aria-labelledby="el-collapse-head-5358" id="el-collapse-content-5358" class="el-collapse-item__wrap" data-old-padding-top="" data-old-padding-bottom="" data-old-overflow="" style="display: none;" aria-hidden="true"><div class="el-collapse-item__content"><div>控制反馈：通过界面样式和交互动效让用户可以清晰的感知自己的操作；</div> <div>页面反馈：操作后，通过页面元素的变化清晰地展现当前状态。</div></div></div></div> <div class="el-collapse-item"><div role="tab" aria-controls="el-collapse-content-3353" aria-describedby="el-collapse-content-3353"><div role="button" id="el-collapse-head-3353" tabindex="0" class="el-collapse-item__header">效率 Efficiency<i class="el-collapse-item__arrow el-icon-arrow-right"></i></div></div><div role="tabpanel" aria-labelledby="el-collapse-head-3353" id="el-collapse-content-3353" class="el-collapse-item__wrap" data-old-padding-top="" data-old-padding-bottom="" data-old-overflow="" style="display: none;" aria-hidden="true"><div class="el-collapse-item__content"><div>简化流程：设计简洁直观的操作流程；</div> <div>清晰明确：语言表达清晰且表意明确，让用户快速理解进而作出决策；</div> <div>帮助用户识别：界面简单直白，让用户快速识别而非回忆，减少用户记忆负担。</div></div></div></div> <div class="el-collapse-item"><div role="tab" aria-controls="el-collapse-content-1547" aria-describedby="el-collapse-content-1547"><div role="button" id="el-collapse-head-1547" tabindex="0" class="el-collapse-item__header">可控 Controllability<i class="el-collapse-item__arrow el-icon-arrow-right"></i></div></div><div role="tabpanel" aria-hidden="true" aria-labelledby="el-collapse-head-1547" id="el-collapse-content-1547" class="el-collapse-item__wrap" style="display: none;"><div class="el-collapse-item__content"><div>用户决策：根据场景可给予用户操作建议或安全提示，但不能代替用户进行决策；</div> <div>结果可控：用户可以自由的进行操作，包括撤销、回退和终止当前操作等。</div></div></div></div></div>',
    type: 2
  }
];
let idGlobal = 8;
let html = "";
export default {
  name: "hello",
  components: {
    draggable
  },
  data() {
    return {
      activeNames: ["1"],
      title: "标题",
      list: message,
      list2: [],
      editable: true,
      isDragging: false,
      delayedDragging: false
    };
  },
  methods: {
    log: function(evt) {
      window.console.log(evt);
    },
    handleChange(val) {
      console.log(val);
    },
    add: function() {
      this.list2.push({
        name: "cat" + idGlobal++,
        id: idGlobal++,
        text:
          "<span class='span-input'><input class='balloon' type='text' placeholder='单行文本' /></span>"
      });
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
.components-list .form-edit-widget-label {
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
.balloon::-webkit-input-placeholder {
  color: #efefef;
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
</style>