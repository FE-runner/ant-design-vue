<template>
  <section class="markdown">
    <h1>{{ isZhCN ? '组件总览' : 'Overview' }}</h1>
    <section class="markdown">
      <p>
        <code>ant-design-vue</code>
        {{
          isZhCN
            ? '为 Web 应用提供了丰富的基础 UI 组件，我们还将持续探索企业级应用的最佳 UI 实践。'
            : 'provides plenty of UI components to enrich your web applications, and we will improve components experience consistently. '
        }}
      </p>
    </section>
    <a-divider></a-divider>
    <a-affix :offset-top="32" @change="handleAffixChange">
      <div
        class="components-overview-affix"
        :class="{ 'components-overview-affixed': searchBarAffixed }"
      >
        <a-input
          ref="inputRef"
          v-model:value="search"
          :placeholder="$t('app.components.overview.search')"
          class="components-overview-search"
          auto-focus
          :style="{ fontSize: searchBarAffixed ? '18px' : '' }"
        >
          <template #suffix>
            <SearchOutlined />
          </template>
        </a-input>
      </div>
    </a-affix>
    <a-divider></a-divider>
    <template v-for="group in menuItems" :key="group.title">
      <div class="components-overview">
        <h2 class="ant-typography components-overview-group-title">
          <a-space align="center">
            {{ isZhCN ? group.title : group.enTitle }}
            <a-tag style="display: block">{{ group.children.length }}</a-tag>
          </a-space>
        </h2>
        <a-row :gutter="[24, 24]">
          <template v-for="component in group.children" :key="component.title">
            <a-col :xs="24" :sm="12" :lg="8" :xl="6">
              <component
                :is="component.target ? 'a' : 'router-link'"
                v-bind="
                  component.target
                    ? { href: component.path, target: component.target }
                    : { to: getLocalizedPathname(component.path, isZhCN) }
                "
              >
                <a-card size="small" class="components-overview-card">
                  <template #title>
                    <div class="components-overview-title">
                      {{ component.title }}
                      {{ isZhCN ? component.subtitle : '' }}
                    </div>
                  </template>
                  <div class="components-overview-img">
                    <img
                      :src="isDark && component.coverDark ? component.coverDark : component.cover"
                      :alt="component.title"
                    />
                  </div>
                </a-card>
              </component>
            </a-col>
          </template>
        </a-row>
      </div>
    </template>
  </section>
</template>
<script lang="ts">
import type { GlobalConfig } from '../App.vue';
import { computed, defineComponent, inject, onMounted, ref } from 'vue';
import { SearchOutlined } from '@ant-design/icons-vue';
import { GLOBAL_CONFIG } from '../SymbolKey';
import useMenus from '../hooks/useMenus';
import { getLocalizedPathname } from '../utils/util';
export default defineComponent({
  name: 'ComponentOverview',
  components: {
    SearchOutlined,
  },
  setup() {
    const globalConfig = inject<GlobalConfig>(GLOBAL_CONFIG);
    const themeMode = inject('themeMode');
    const isDark = computed<boolean>(() => (themeMode as any).theme.value === 'dark');
    const search = ref('');
    const inputRef = ref();
    const { dataSource } = useMenus();

    const searchBarAffixed = ref(false);
    function handleAffixChange(affixed?: boolean) {
      searchBarAffixed.value = affixed;
    }

    const menuItems = computed(() => {
      return [
        // {
        //   children: [
        //     {
        //       category: 'Components',
        //       cols: 1,
        //       cover: 'https://gw.alipayobjects.com/zos/alicdn/f-SbcX2Lx/Table.svg',
        //       coverDark: 'https://gw.alipayobjects.com/zos/alicdn/f-SbcX2Lx/Table.svg',
        //       path: 'https://surely.cool/',
        //       subtitle: '更强大的表格',
        //       title: 'Surely Table',
        //       type: 'Advanced And Powerful',
        //       target: '_blank',
        //     },
        //     {
        //       category: 'Components',
        //       cols: 1,
        //       cover: 'https://aliyuncdn.antdv.com/form/static/assets/landing-config.4f9d5425.png',
        //       coverDark:
        //         'https://aliyuncdn.antdv.com/form/static/assets/landing-config.4f9d5425.png',
        //       path: 'https://form.antdv.com/',
        //       subtitle: '在线表单',
        //       title: 'Surely Form',
        //       type: 'Advanced And Powerful',
        //       target: '_blank',
        //     },
        //   ],
        //   enTitle: 'Advanced And Powerful',
        //   title: '更强大',
        //   order: 0,
        // },
      ].concat(
        dataSource.value
          .filter(i => i.order > -1)
          .map(group => {
            const components = group.children.filter(
              (component: any) =>
                !search.value.trim() ||
                component.title.toLowerCase().includes(search.value.trim().toLowerCase()) ||
                (component.subtitle || '')
                  .toLowerCase()
                  .includes(search.value.trim().toLowerCase()),
            );
            return { ...group, children: components };
          })
          .filter(i => i.children.length),
      );
    });
    onMounted(() => {
      inputRef.value.focus();
    });
    return {
      globalConfig,
      search,
      menuItems,
      getLocalizedPathname,
      inputRef,
      isZhCN: globalConfig?.isZhCN,
      isDark,
      searchBarAffixed,
      handleAffixChange,
    };
  },
});
</script>
<style lang="less" src="./ComponentOverview.less"></style>
