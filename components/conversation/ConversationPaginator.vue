<script setup lang="ts">
import type { Paginator, mastodon } from 'masto'

const { paginator } = defineProps<{
  paginator: Paginator<mastodon.v1.Conversation[], mastodon.DefaultPaginationParams>
}>()

function preprocess(items: mastodon.v1.Conversation[]): mastodon.v1.Conversation[] {
  return items.filter(items => !items.lastStatus?.filtered?.find(
    filter => filter.filter.filterAction === 'hide' && filter.filter.context.includes('thread'),
  ))
}
</script>

<template>
  <CommonPaginator :paginator="paginator" :preprocess="preprocess">
    <template #default="{ item }">
      <ConversationCard
        :conversation="item"
        border="b base" py-1
      />
    </template>
  </CommonPaginator>
</template>
