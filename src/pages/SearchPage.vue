<template>
  <div>
    <van-search v-model="search" placeholder="请输入搜索内容" />
    <div style="font-size: 20px; font-weight: bold; margin-bottom: 10px">
      已选择的标签：
      <van-row gutter="16">
        <van-col v-for="tag in selectedTags" :key="tag">
          <van-tag :color="getTagColor(tag)" @click="removeTag(tag)">
            {{ tag }}
            <van-icon name="cross" />
          </van-tag>
        </van-col>
      </van-row>
    </div>
    <van-divider />
    <van-row gutter="16">
      <van-col v-for="tag in filteredTags" :key="tag.name">
        <van-tag :color="tag.color" @click="selectTag(tag.name)">
          {{ tag.name }}
        </van-tag>
      </van-col>
    </van-row>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
// import { Tag, Search, Icon, Divider, Row, Col } from 'vant';

// 标签数据
const tags = ref([
  { name: '大一', color: 'blue', category: '身份' },
  { name: '大二', color: 'blue', category: '身份' },
  { name: '大三', color: 'blue', category: '身份' },
  { name: '大四', color: 'blue', category: '身份' },
  { name: '男', color: 'red', category: '性别' },
  { name: '女', color: 'red', category: '性别' },
]);

// 搜索词
const search = ref('');

// 已选择的标签
const selectedTags = ref([]);

// 过滤后的标签，不包括已选择的标签
const filteredTags = computed(() => {
  if (!search.value) {
    return tags.value.filter((tag) => !selectedTags.value.includes(tag.name));
  }
  return tags.value
    .filter((tag) => tag.name.includes(search.value))
    .filter((tag) => !selectedTags.value.includes(tag.name));
});

// 选择标签
const selectTag = (tagName) => {
  if (!selectedTags.value.includes(tagName)) {
    selectedTags.value.push(tagName);
  }
  search.value = '';
};

// 移除标签
const removeTag = (tagName) => {
  const index = selectedTags.value.indexOf(tagName);
  if (index !== -1) {
    selectedTags.value.splice(index, 1);
  }
};

// 获取标签颜色
const getTagColor = (tagName) => {
  const tag = tags.value.find((t) => t.name === tagName);
  return tag ? tag.color : '';
};
</script>

<style>
/* 这里可以添加你的样式 */
</style>
