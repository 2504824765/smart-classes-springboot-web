<script setup lang="tsx">
import { ContentWrap } from '@/components/ContentWrap'
import { useI18n } from '@/hooks/web/useI18n'
import { Table, TableColumn } from '@/components/Table'
import { getStudentListApi } from '@/api/student'
import { TableData } from '@/api/table/types'
import { ref, h, onMounted } from 'vue'
import { ElTag } from 'element-plus'
import { BaseButton } from '@/components/Button'
import { useRouter } from 'vue-router'

interface Params {
  pageIndex?: number
  pageSize?: number
}

const { t } = useI18n()
const router = useRouter()

const columns: TableColumn[] = [
  {
    field: 'id',
    label: t('student.id')
  },
  {
    field: 'name',
    label: t('student.name')
  },
  {
    field: 'gender',
    label: t('student.gender')
  },
  {
    field: 'dept',
    label: t('student.dept'),
    sortable: true
  },
  {
    field: 'gpa',
    label: t('student.gpa')
  },
  {
    field: 'action',
    label: t('tableDemo.action'),
    slots: {
      default: (data) => {
        return (
          <BaseButton type="primary" onClick={() => actionFn(data)}>
            {t('tableDemo.action')}
          </BaseButton>
        )
      }
    }
  }
]

const loading = ref(true)

const tableDataList = ref<TableData[]>([])

const getTableList = async (params?: Params) => {
  const res = await getStudentListApi(
    params || {
      pageIndex: 1,
      pageSize: 10
    }
  )
    .catch(() => {})
    .finally(() => {
      loading.value = false
    })
  if (res) {
    console.log(res.data)
    tableDataList.value = res.data
  }
}

onMounted(() => {
  getTableList()
})

const actionFn = (data: any) => {
  console.log(data)
}

const goToVideoPlay = () => {
  router.push('/SmartClass/VideoPlay')
}
</script>

<template>
  <el-text class="mx-1" size="large">这是 教师-学生管理 页面</el-text>
  <ContentWrap :title="t('tableDemo.table')" :message="t('tableDemo.tableDes')">
    <BaseButton type="primary" @click="goToVideoPlay" style="margin-bottom: 16px">
      视频播放
    </BaseButton>
    <BaseButton type="primary" @click="getTableList" style="margin-bottom: 16px">刷新</BaseButton>
    <Table
      :columns="columns"
      :data="tableDataList"
      :loading="loading"
      :defaultSort="{ prop: 'display_time', order: 'descending' }"
    />
  </ContentWrap>
</template>
