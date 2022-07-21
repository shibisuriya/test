<template>
  <div>
    <SectionEditor
      v-if="showSectionEditor"
      :section="sectionSelectedForEditing"
      @updateSection="updateSection"
      @close="
        showSectionEditor = false;
        sectionSelectedForEditing = null;
      "
    ></SectionEditor>
    <div>
      {{ layout }}
    </div>
    <Gridstack
      ref="gridstack"
      :dragHandle="'.widget-header'"
      :column="96"
      :rowHeight="'15px'"
      :layout.sync="layout"
      :float="false"
    >
      <GridstackItem
        v-for="widget in layout"
        :noScroll="Boolean(widget.children)"
        :key="widget.id"
        :id="widget.id"
        :w="widget.w"
        :h="widget.h"
        :x="widget.x"
        :y="widget.y"
      >
        <SectionItem
          v-if="widget.children"
          @remove="removeWidget(widget.id)"
          @addWidget="addWidget(widget.id)"
          @editSection="editSection(widget.id)"
          @resizeSection="resizeSection"
          :section="widget"
        >
          <GridstackSection :id="widget.id" :section="widget">
            <GridstackItem
              v-for="child in widget.children"
              :key="child.id"
              :id="child.id"
              :parentId="widget.id"
              :w="child.w"
              :h="child.h"
              :x="child.x"
              :y="child.y"
            >
              <div>
                <h1>Hello</h1>
              </div>
            </GridstackItem>
          </GridstackSection>
        </SectionItem>
        <div v-else>
          <SampleWidget @remove="removeWidget(widget.id)">Content</SampleWidget>
        </div>
      </GridstackItem>
    </Gridstack>
  </div>
</template>

<script>
import SectionItem from "./SectionItem.vue";
import SectionEditor from "./SectionEditor.vue";
import { Gridstack, GridstackSection } from "@facilio/ui/gridlayout";
import GridstackItem from "./GridstackItem.vue";
export default {
  components: {
    SectionItem,
    Gridstack,
    GridstackItem,
    GridstackSection,
    SectionEditor,
  },
  methods: {
    updateSection(data) {
      const index = this.layout.findIndex((s) => s.id == data.id);
      this.layout[index] = data.section;
      console.log(data);
    },
    editSection(id) {
      const index = this.layout.findIndex((s) => s.id == id);
      this.sectionSelectedForEditing = this.layout[index];
      this.showSectionEditor = true;
    },
    resizeSection({ id, height, collapsed }) {
      const index = this.layout.findIndex((w) => w.id == id);
      const section = this.layout[index];
      // const children = section.children;
      // const maxHeight = this.findSectionMaxHeight(children);
      section.h = height;
      section.collapsed = collapsed;
      this.layout[index] = section;
    },
  },
  data() {
    return {
      sectionSelectedForEditing: null,
      showSectionEditor: false,
      layout: [
        {
          id: "1657767164721",
          x: 0,
          y: 2,
          w: 96,
          h: 20,
          name: "My name is...",
          desc: "My purpose is...",
          minW: 96,
          maxW: 96,
          collapsed: false,
          data: { sample: 1 },
          children: [
            {
              id: "1657767185305",
              w: 50,
              h: 20,
              x: 2,
              y: 3,
              data: { sample: 2 },
            },
            {
              id: "1657788343941",
              w: 7,
              h: 20,
              x: 50,
              y: 0,
              data: { sample: 3 },
            },
          ],
        },
        // {
        //   id: "1657788332370",
        //   x: 0,
        //   y: 0,
        //   w: 96,
        //   name: "My name is...",
        //   desc: "My purpose is...",
        //   minW: 96,
        //   maxW: 96,
        //   h: 25,
        //   collapsed: false,
        //   appearance: {},
        //   children: [
        //     {
        //       id: "1657767175263",
        //       w: 50,
        //       h: 5,
        //       x: 2,
        //       y: 0,
        //       data: { sample: 5 },
        //     },
        //     {
        //       id: "1657788338182",
        //       w: 50,
        //       h: 5,
        //       x: 6,
        //       y: 0,
        //       data: { sample: 6 },
        //     },
        //   ],
        // },
        { id: "1657767170573", w: 40, h: 10, x: 4, y: 4, data: { sample: 8 } },
      ],
    };
  },
};
</script>

<style lang="scss">
@use "sass:math";
.grid-stack-item-content {
  border: 1px solid black;
}
.grid-stack {
  border: 1px solid black;
}
.grid-stack > .grid-stack-item {
  $gridstack-columns: 96;

  min-width: math.div(100%, $gridstack-columns);

  @for $i from 0 through $gridstack-columns {
    &[gs-w="#{$i}"] {
      width: math.div(100%, $gridstack-columns) * $i;
    }
    &[gs-x="#{$i}"] {
      left: math.div(100%, $gridstack-columns) * $i;
    }
    &[gs-min-w="#{$i}"] {
      min-width: math.div(100%, $gridstack-columns) * $i;
    }
    &[gs-max-w="#{$i}"] {
      max-width: math.div(100%, $gridstack-columns) * $i;
    }
  }
}
</style>
