<template>
  <el-card
    class="course-card"
    shadow="hover"
    @click="goToCourseDetail"
  >
    <el-image
      :src="course.image"
      alt="课程封面"
      style="width: 100%; height: 160px; object-fit: cover"
    />
    <div class="course-content">
      <el-text tag="h3" size="large">{{ course.name }}</el-text>
      <el-text type="info" class="course-desc">
        {{ course.description }}
      </el-text>
      <el-text
        class="status"
        type="success"
        size="small"
        v-if="course.open"
      >
        开放中
      </el-text>
      <el-text
        class="status"
        type="danger"
        size="small"
        v-else
      >
        未开放
      </el-text>
    </div>
  </el-card>
</template>

<script setup lang="ts">
import { defineProps } from 'vue'

const props = defineProps<{
  course: {
    id: string | number
    name: string
    image: string
    description: string
    open: boolean
  }
}>()

const goToCourseDetail = () => {
    //带实现
}
</script>

<style scoped>
.course-card {
  cursor: pointer;
  width: 300px;
  border-radius: 12px;
  overflow: hidden;
  transition: transform 0.2s ease;
}
.course-card:hover {
  transform: translateY(-4px);
}
.course-content {
  margin-top: 12px;
}
.course-desc {
  display: block;
  margin: 8px 0;
  font-size: 14px;
  color: #666;
}
.status {
  display: block;
  margin-top: 4px;
}
</style>
