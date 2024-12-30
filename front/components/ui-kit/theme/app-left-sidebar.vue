<template>
  <div class="h-100 display-flex flex-column">
    <div class="flex-center-vertical">
      <app-icon :icon="TablerIconConstants.dashboard" :size="20"  />
      Home
    </div>


    <div class="flex-center-vertical">
      <app-icon :icon="TablerIconConstants.transaction" :size="20" />
      Transactions
    </div>

    <div class="flex-center-vertical">
      <app-icon :icon="TablerIconConstants.transaction" :size="20" />
      Transactions
    </div>

    <div class="flex-center-vertical">
      <app-icon :icon="TablerIconConstants.transaction" :size="20" />
      Transactions
    </div>
  </div>

</template>

<script setup>
import { useDataStore } from '~/stores/dataStore'
import { useProfileStore } from '~/stores/profileStore'
import RouteConstants from '~/constants/RouteConstants'
import TablerIconConstants from '~/constants/TablerIconConstants'
import anime from 'animejs'
import { animateBottomToolbarAddButton } from '~/utils/AnimationUtils.js'

const dataStore = useDataStore()
const profileStore = useProfileStore()
const appStore = useAppStore()
const route = useRoute()

const tabConstants = {
  add: 'add',
  transactionList: 'transactionList',
  dashboard: 'dashboard',
  transactionTemplateList: 'transactionTemplateList',
  extras: 'extras',
  settings: 'settings',
}
const activeTab = ref(null)

const { isKeyboardVisible } = useKeyboard()

watch(
    () => route.path,
    (newValue) => {
      if (newValue === RouteConstants.ROUTE_TRANSACTION_LIST || RouteConstants.isForm(RouteConstants.ROUTE_TRANSACTION_ID, newValue)) {
        activeTab.value = tabConstants.transactionList
      }

      if (newValue === RouteConstants.ROUTE_TRANSACTION_TEMPLATE_LIST || RouteConstants.isForm(RouteConstants.ROUTE_TRANSACTION_TEMPLATE_ID, newValue)) {
        activeTab.value = tabConstants.transactionTemplateList
      }

      if (
          [RouteConstants.ROUTE_EXTRAS, RouteConstants.ROUTE_TAG_LIST, RouteConstants.ROUTE_ACCOUNT_LIST, RouteConstants.ROUTE_CATEGORY_LIST].includes(newValue) ||
          RouteConstants.isForm(RouteConstants.ROUTE_TAG_ID, newValue) ||
          RouteConstants.isForm(RouteConstants.ROUTE_ACCOUNT_ID, newValue) ||
          RouteConstants.isForm(RouteConstants.ROUTE_CATEGORY_ID, newValue)
      ) {
        activeTab.value = tabConstants.extras
      }

      if ([RouteConstants.ROUTE_SETTINGS].includes(newValue)) {
        activeTab.value = tabConstants.settings
      }
    },
    { deep: true, immediate: true },
)

onMounted(async () => {})

const getStrokeWidth = (active) => {
  return active ? 2.2 : 1.7
}

const onNewTransaction = () => {
  animateBottomToolbarAddButton()
  onChange(tabConstants.add)
}

const onChange = async (code) => {
  switch (code) {
    case tabConstants.dashboard:
      await navigateTo(RouteConstants.ROUTE_DASHBOARD)
      break

    case tabConstants.add:
      await navigateTo(RouteConstants.ROUTE_TRANSACTION_ID)
      break

    case tabConstants.transactionList:
      await navigateTo(RouteConstants.ROUTE_TRANSACTION_LIST)
      break

    case tabConstants.transactionTemplateList:
      await navigateTo(RouteConstants.ROUTE_TRANSACTION_TEMPLATE_LIST)
      break

    case tabConstants.extras:
      await navigateTo(RouteConstants.ROUTE_EXTRAS)
      break

    case tabConstants.settings:
      await navigateTo(RouteConstants.ROUTE_SETTINGS)
      break
  }
}
</script>
