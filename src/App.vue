<template>

  <div style="position: relative; margin: 50px;" class="select-cascader" ref="dom">
    {{options}}
    <a-select
      labelInValue
      class="select"
      @focus="focus"
      mode="multiple"
      :getPopupContainer="triggerNode => triggerNode.parentNode"
      v-model="data"
      dropdownClassName="select-dropdown"
    >
    </a-select>
    <div v-show="popupVisible" v-click-outside="hide" @click="toggle" class="ant-cascader-menus ant-cascader-menus-placement-bottomLeft">
      <div class="ant-cascader-menus-content">
        <ul class="ant-cascader-menu" v-for="(item, index) in options" :key="index">
          <li :title="menu.label"
              v-for="menu in item"
              :key="menu.value"
              @click="showNext(index, menu)"
              :class="{
                'ant-cascader-menu-item-expand': index < options.length - 1,
                'ant-cascader-menu-item-active': hi(index, menu)
              }"
              class="ant-cascader-menu-item ">{{menu.label}}
          </li>
        </ul>
      </div>
    </div>
    {{data}}

  </div>
  <!--<div style="margin: 50px">-->
  <!--&lt;!&ndash;<a-tree-select&ndash;&gt;-->
  <!--&lt;!&ndash;showSearch&ndash;&gt;-->
  <!--&lt;!&ndash;style="width: 300px"&ndash;&gt;-->
  <!--&lt;!&ndash;:value="value"&ndash;&gt;-->
  <!--&lt;!&ndash;:dropdownStyle="{ maxHeight: '400px', overflow: 'auto' }"&ndash;&gt;-->
  <!--&lt;!&ndash;:treeData="treeData"&ndash;&gt;-->
  <!--&lt;!&ndash;placeholder='Please select'&ndash;&gt;-->
  <!--&lt;!&ndash;allowClear&ndash;&gt;-->
  <!--&lt;!&ndash;labelInValue&ndash;&gt;-->
  <!--&lt;!&ndash;multiple&ndash;&gt;-->
  <!--&lt;!&ndash;treeDefaultExpandAll&ndash;&gt;-->
  <!--&lt;!&ndash;@change="onChange"&ndash;&gt;-->
  <!--&lt;!&ndash;@search="onSearch"&ndash;&gt;-->
  <!--&lt;!&ndash;@select="onSelect"&ndash;&gt;-->
  <!--&lt;!&ndash;&gt;&ndash;&gt;-->
  <!--&lt;!&ndash;&lt;!&ndash;<a-tree-select-node value='parent 1' title='parent 1' key='0-1'>&ndash;&gt;&ndash;&gt;-->
  <!--&lt;!&ndash;&lt;!&ndash;<a-tree-select-node value='parent 1-0' title='parent 1-0' key='0-1-1'>&ndash;&gt;&ndash;&gt;-->
  <!--&lt;!&ndash;&lt;!&ndash;<a-tree-select-node value='leaf1' title='my leaf' key='random'/>&ndash;&gt;&ndash;&gt;-->
  <!--&lt;!&ndash;&lt;!&ndash;<a-tree-select-node value='leaf2' title='your leaf' key='random1'/>&ndash;&gt;&ndash;&gt;-->
  <!--&lt;!&ndash;&lt;!&ndash;</a-tree-select-node>&ndash;&gt;&ndash;&gt;-->
  <!--&lt;!&ndash;&lt;!&ndash;<a-tree-select-node value='parent 1-1' title='parent 1-1' key='random2'>&ndash;&gt;&ndash;&gt;-->
  <!--&lt;!&ndash;&lt;!&ndash;<a-tree-select-node value='sss' key='random3'>&ndash;&gt;&ndash;&gt;-->
  <!--&lt;!&ndash;&lt;!&ndash;<b style="color: #08c" slot="title">sss</b>&ndash;&gt;&ndash;&gt;-->
  <!--&lt;!&ndash;&lt;!&ndash;</a-tree-select-node>&ndash;&gt;&ndash;&gt;-->
  <!--&lt;!&ndash;&lt;!&ndash;</a-tree-select-node>&ndash;&gt;&ndash;&gt;-->
  <!--&lt;!&ndash;&lt;!&ndash;</a-tree-select-node>&ndash;&gt;&ndash;&gt;-->
  <!--&lt;!&ndash;</a-tree-select>&ndash;&gt;-->
  <!--</div>-->

</template>

<script>
import { Vue, Component } from 'vue-property-decorator';
import ClickOutside from 'vue-click-outside';


@Component({
  directives: {
    ClickOutside,
  },
})
export default class SelectCascaders extends Vue {
    popupVisible = false;
    data = [
      { value: ['0', '1', '2'], key: 'Zhejiang/Hangzhou/West Lake' },
      { value: ['3', '4', '5'], key: 'Jiangsu/Nanjing/Zhong Hua Men' },
    ];
    selectValue = {
      value: [],
      key: [],
    };
    options = [
      [{ value: '0',
        label: 'Zhejiang',
        children: [{
          value: '1',
          label: 'Hangzhou',
          children: [
            {
              value: '2',
              label: 'West Lake',
            },
            {
              value: '2-1',
              label: 'West Lake1',
            },
          ],
        }] }, { value: '3',
        label: 'Jiangsu',
        children: [
          {
            value: '4',
            label: 'Nanjing',
            children: [
              {
                value: '5',
                label: 'Zhong Hua Men',
              },
            ],
          },
        ] }],
      [],
      [],
    ];
    value = [{
      label: 'Child Node1',
      value: '0-0-0',
    }];

    focus() {
      this.popupVisible = true;
    }
    hide() {
      this.popupVisible = false;
    }
    toggle() {
      this.popupVisible = true;
    }

    showNext(index, item) {
      // 点击的是最后一级
      Vue.set(this.selectValue.value, index, item.value);
      Vue.set(this.selectValue.key, index, item.label);
      if (index >= this.options.length - 1) {
        if (!item.selected) {
          this.data.push({
            value: this.selectValue.value,
            key: this.selectValue.key.join('/'),
          });
        }
        return;
      }
      this.options.forEach((item2, index2) => {
        if (index2 > index) {
          Vue.set(this.options, index2, []);
        }
      });
      Vue.set(this.options, index + 1, item.children);
    }

    hi(index, menu) {
      if (this.data.find(item => item.value[index] === menu.value)) {
        menu.selected = true;
        return true;
      }
    }
}
</script>

<style>
  .select-cascader {
    margin: 50px;
    width: 340px;
    position: relative;
  }

  .cascader {
    left: 0;
    top: 0;
    z-index: -1;
    width: 100%;
  }

  .select {
    width: 340px;
  }

  .ant-select-dropdown {
    display: none;
  }
</style>
