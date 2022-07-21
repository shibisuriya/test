<template>
  <el-dialog
    title="Section editor"
    :visible.sync="dialogVisible"
    style="background-color: 1em"
    width="50%"
  >
    <el-input
      v-model="sectionName"
      placeholder="Section Name"
      style="margin-bottom: 1em"
    />
    <el-input v-model="sectionDesc" placeholder="Section Description" />
    <div>
      <h1>Debug</h1>
      {{ currentTile }}
    </div>
    <div class="tile-container">
      <div
        v-for="(tile, index) in tiles"
        :key="index"
        class="tile"
        @click="
          () => {
            currentTile = tile;
          }
        "
        :class="{ currenTile: currentTile?.tileName == tile?.tileName }"
      >
        <img src="./tile_images/default.png" alt="" />
        <h3>{{ tile.tileName }}</h3>
      </div>
    </div>
    <div v-if="currentTile?.tileName == 'Banner'" style="margin: 2em auto">
      <el-upload
        class="upload-demo"
        drag
        action="https://jsonplaceholder.typicode.com/posts/"
        :on-preview="handlePreview"
        :on-remove="handleRemove"
        :file-list="fileList"
        multiple
      >
        <i class="el-icon-upload"></i>
        <div class="el-upload__text">
          Drop file here or <em>click to upload</em>
        </div>
        <div class="el-upload__tip" slot="tip">
          jpg/png files with a size less than 500kb
        </div>
      </el-upload>
    </div>
    <div
      v-if="currentTile?.tileName == 'Emphasis'"
      class="color-picker-container flex-container"
    >
      <div>
        <el-color-picker
          v-model="currentTile.color"
          show-alpha
        ></el-color-picker>
      </div>
      <div>
        <el-color-picker
          v-model="currentTile.backgroundColor"
          show-alpha
        ></el-color-picker>
      </div>
    </div>
    <span
      slot="footer"
      class="flex-container"
      style="margin: 0 auto; width: 30%"
    >
      <el-button @click="close" style="display: block">Cancel</el-button>
      <el-button type="primary" @click="confirm" style="display: block"
        >Confirm</el-button
      >
    </span>
  </el-dialog>
</template>

<script>
import _ from "lodash";
export default {
  mounted() {
    const defaulBanner = {
      tileName: "Default",
      tileImage: "./tile_images/banner.png",
    };
    this.sectionCopy = _.cloneDeep(this.section);
    this.currentTile = this.sectionCopy?.banner_meta
      ? this.sectionCopy?.banner_meta
      : defaulBanner;
    this.sectionName = this.sectionCopy?.name;
    this.sectionDesc = this.sectionCopy?.desc;
    // if (this.sectionCopy.banner_meta.type) {
    //   this.currentTile.name = this.sectionCopy.banner_meta.type;
    // } else {
    //   this.currentTile.name = "Default";
    // }
    // this.sectionName = this.sectionCopy?.name;
    // this.sectionDesc = this.sectionCopy?.desc;
    // console.log(this.sectionCopy);
  },
  data() {
    return {
      sectionCopy: null,
      dialogVisible: true,
      sectionDesc: null,
      sectionName: null,
      currentTile: null,
      color: null,
      backgroundColor: null,
      tiles: [
        {
          tileName: "Default",
          bgImage: "./tile_images/banner.png",
        },
        {
          tileName: "Emphasis",
          bgImage: "./tile_images/emphasis.png",
        },
        {
          tileName: "Banner",
          bgImage: "./tile_images/banner.png",
        },
      ],
    };
  },
  props: ["section"],
  methods: {
    onUpload() {},
    onUploadSuccess() {},
    onPhotoDelete() {},
    close() {
      this.$emit("close");
    },
    handlePreview() {},
    handleRemove() {},
    confirm() {
      this.sectionCopy.desc = this.sectionDesc;
      this.sectionCopy.name = this.sectionName;
      // Remove uncessary properties from currentTile object.
      delete this.currentTile.bgImage;
      this.sectionCopy.banner_meta = this.currentTile;
      this.$emit("updateSection", {
        id: this.sectionCopy.id,
        section: this.sectionCopy,
      });
      this.close();
    },
  },
};
</script>
<style lang="css" scoped>
.center {
  margin: 0 auto;
  display: block;
}
/* .upload-demo {
    display: flex;
    justify-content: center;
} */

.tile-container {
  display: flex;
  justify-content: space-between;
}
.flex-container {
  display: flex;
  justify-content: space-evenly;
}
.color-picker-container {
  margin: 3em auto 1em auto;
  display: flex;
  justify-content: space-evenly;
  width: 50%;
}

.tile {
  border: 1px solid black;
  padding: 1em;
  margin-left: 1em;
  margin-right: 1em;
  border-radius: 4px;
}
.el-upload {
  display: block;
  margin: 0 auto !important;
}
.tile:hover {
  border: 3px solid rgb(0, 107, 194);
  background-color: none;
}

.el-color-picker__inner {
  width: 100%;
  display: block;
}

.currenTile {
  border: 3px solid rgb(0, 107, 194) !important;
  color: rgb(0, 107, 194) !important;
  background-color: rgb(0, 107, 194, 0.1) !important;
}
.currenTile > img {
  box-shadow: 0 3px 10px rgb(0 0 0 / 0.2);
}
img {
  width: 100%;
}
h2,
h3 {
  text-align: center;
}
</style>
