<template>
  <div>
    <el-row :style="accordionHeadStyle">
      <el-col :span="2" class="mt35">
        <i class="el-icon-arrow-right ml35"  @click="resizeSection"></i>
      </el-col>
      <el-col :span="18" style="text-align: center">
        <div>{{ section.name }}</div>
        <div>{{ section.desc }}</div>
      </el-col>
      <el-col :span="4" class="mt35">
        <el-row>
          <el-col :span="8">
            <el-dropdown @command="addWidget">
              <i class="el-icon-plus"></i>
              <el-dropdown-menu slot="dropdown">
                <el-dropdown-item command="addText">Add Text</el-dropdown-item>
                <el-dropdown-item command="addImage"
                  >Add Image</el-dropdown-item
                >
                <el-dropdown-item command="addComponent"
                  >Add Component</el-dropdown-item
                >
                <el-dropdown-item command="addCard">Add Card</el-dropdown-item>
              </el-dropdown-menu>
            </el-dropdown>
          </el-col>
          <el-col :span="8">
            <i
              class="el-icon-delete fc-delete-icon pointer"
              @click="removeSection"
            ></i>
          </el-col>
          <el-col :span="8">
            <i class="el-icon-edit" @click="editSection"></i>
          </el-col>
        </el-row>
      </el-col>
    </el-row>
    <div>
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  props: ["section"],
  methods: {
    removeSection() {
      this.$emit("removeSection");
    },
    editSection() {
      this.$emit("editSection");
    },
    resizeSection() {
      const accordionHeadHeight = 7; // Hard code will refactor later.
      if (this.section.collapsed) {
        this.$emit("resizeSection", {
          id: this.section.id,
          height: accordionHeadHeight,
          collapsed: !this.section.collapsed,
        });
      } else {
        const accordionBodyHeight = this.findSectionMaxHeight(
          this.section.children
        );
        this.$emit("resizeSection", {
          id: this.section.id,
          height: 7 + accordionBodyHeight,
          collapsed: !this.section.collapsed,
        });
      }
    },
    addWidget() {
      this.emit("addWidget");
    },
    findSectionMaxHeight(children) {
      const maxHeight = Math.max(...children.map((o) => o.y + o.h));
      return maxHeight;
    },
  },
  data() {
    return {
      cellHeight: 15,
    };
  },
  computed: {
    accordionHeadStyle() {
      return {
        height: "83px", // Hard code for now, will refactor later.
        maxHeight: "83px", // Hard code for now, will refactor later.
        "background-color": this.section?.banner_meta?.backgroundColor
          ? this.section?.banner_meta?.backgroundColor
          : "white",
        color: this.section?.banner_meta?.color
          ? this.section?.banner_meta?.color
          : "black",
        // "background-image":
        //   'url("https://encrypted-tbn1.gstatic.com/images?q=tbn:ANd9GcQb8fXEwjltk4U35N3evR9h81MqZLNRMFVZJdx3noqWPbAqnPzc")',
      };
    },
  },
};
</script>

<style lang="css" scoped>
h2,
h3 {
  text-align: center;
}
.mt35 {
  margin-top: 35px;
}
.ml35 {
  margin-left: 35px;
}
</style>
