<template>
  <SiderWrapper
    v-show="getShowSideBar"
    :class="prefixCls"
    :width="getWidth"
    :inverted="getInverted"
  >
    <template #header>
      <AppLogo
        v-if="getIsSidebarType"
        class="*vs-layout h-64px"
        :collapsed="getCollapsed"
        :title-color="getMenuInverted ? 'white' : undefined"
      />
    </template>

    <template #default>
      <LayoutMenu :menu-mode="getMode" :split-type="getSplitType" :inverted="getInverted" />
    </template>
  </SiderWrapper>
</template>

<script setup lang="ts">
  import { computed } from 'vue';
  import { clamp } from 'lodash-es';
  import { MenuModeEnum, MenuSplitTypeEnum } from '/@/enums/menuEnum';

  import SiderWrapper from './SiderWrapper.vue';
  import LayoutMenu from '../menu/index.vue';

  import { useDesign } from '/@/composables/web/useDesign';
  import { useAppInject } from '/@/composables/web/useAppInject';
  import { useMenuSetting } from '/@/composables/setting/useMenuSetting';

  const { prefixCls } = useDesign('layout-sideBar');
  const { getIsMobile } = useAppInject();

  const { getCollapsed, getRealWidth, getMenuHidden, getMenuInverted, getIsSidebarType, getSplit } =
    useMenuSetting();

  const getSplitType = computed(() => {
    return getSplit.value ? MenuSplitTypeEnum.SECONDARY : MenuSplitTypeEnum.NONE;
  });

  const getMode = computed(() => {
    return getSplit.value ? MenuModeEnum.VERTICAL : null;
  });

  const getShowSideBar = computed(() => {
    return getSplit.value ? !getMenuHidden.value : true;
  });

  const getInverted = computed(() => getMenuInverted.value && !getSplit.value);
  const getWidth = computed(() =>
    getIsMobile.value ? clamp(100, getRealWidth.value, 240) : getRealWidth.value
  );
</script>
