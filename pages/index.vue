<script lang="ts" setup>
useHead({
  title: 'Dashboard',
})
useSeoMeta({
  ogTitle: 'Dashboard',
  ogDescription: 'There are challenges in finding crucial technical details and comparing various privacy-focused projects.',
  ogImage: '/web3privacy_eye.webp',
})
import { format } from 'date-fns'
const { groupedProjectsPerCategory, lastUpdated } = storeToRefs(useData())
const formattedLastUpdated = computed(() =>
  lastUpdated.value
    ? format(new Date(lastUpdated.value), 'PPpp')
    : '',
)

const isOpen = ref(false)
const route = useRoute()

onMounted(() => {
  if (route.query.status === 'success')
    isOpen.value = true
})
</script>

<template>
  <div>
    <Dialog
      v-model="isOpen"
      heading="Pull Request Submitted for Approval"
      desc="Your pull request has been submitted! It will need to be manually reviewed and approved by a team member before merging. You can track its status on GitHub."
      cta="View on GitHub"
      :to="route.query.pr as string"
    />
    <div v-if="formattedLastUpdated" class="text-right text-xs opacity-70 mb-4">
      Last Updated: {{ formattedLastUpdated }}
    </div>
    <ProjectGrid :projects="groupedProjectsPerCategory" />
  </div>
</template>
