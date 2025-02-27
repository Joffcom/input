<template>
  <app-layout title="Form Settings">
    <div class="mx-auto w-full max-w-5xl px-4">
      <FormSummary
        class="mt-6"
        v-bind="{ form, blocks: store.blocks || undefined }"
      />

      <TabGroup
        :vertical="true"
        as="div"
        class="mx-auto mt-4 grid w-full grid-cols-12 gap-x-6"
        :selectedIndex="selectedTabIndex"
        @change="tabChanged"
      >
        <div class="col-span-4 pt-8">
          <TabList
            as="nav"
            class="divide-y divide-grey-50 overflow-hidden rounded border border-grey-200 bg-white"
            aria-label="Sidebar"
          >
            <Tab
              v-for="item in navigation"
              v-slot="{ selected }"
              :key="item.name"
              as="template"
            >
              <button
                :class="[
                  selected
                    ? 'bg-grey-50'
                    : 'text-grey-600 hover:bg-grey-100 hover:text-grey-900',
                  'relative block w-full px-3 py-3 text-left text-sm font-medium',
                ]"
              >
                <div
                  v-show="selected"
                  class="absolute inset-y-0 left-0 w-1 bg-blue-400"
                />
                <span class="truncate">{{ item.name }}</span>
              </button>
            </Tab>
          </TabList>
        </div>

        <TabPanels as="div" class="col-span-8">
          <TabPanel v-for="item in navigation" :key="item.name">
            <h3 class="border-b border-grey-300 pb-2 text-xl font-medium">
              {{ item.name }}
            </h3>

            <div class="mt-6">
              <component :is="item.component" />
            </div>
          </TabPanel>
        </TabPanels>
      </TabGroup>
    </div>
  </app-layout>
</template>

<script setup lang="ts">
import AppLayout from "@/Layouts/AppLayout.vue";
import { TabGroup, TabList, Tab, TabPanels, TabPanel } from "@headlessui/vue";
import Appearance from "@/components/Factory/Settings/Appearance.vue";
import Delete from "@/components/Factory/Settings/Delete.vue";
import FormSummary from "@/components/Factory/FormSummary.vue";
import Options from "@/components/Factory/Settings/Options.vue";
import Privacy from "@/components/Factory/Settings/Privacy.vue";
import TemplateExport from "@/components/Factory/Settings/TemplateExport.vue";
import Embed from "@/components/Factory/Settings/Embed.vue";
import { useForm } from "@/stores";
import { onMounted, onUnmounted, ref } from "vue";

const props = defineProps<{
  form: FormModel;
}>();
const store = useForm();

const selectedTabIndex = ref(0);

const navigation: Array<{
  name: string;
  component: any;
  slug: string;
}> = [
  { name: "Options", component: Options, slug: "options" },
  { name: "Privacy", component: Privacy, slug: "privacy" },
  { name: "Appearance", component: Appearance, slug: "appearance" },
  { name: "Embed", component: Embed, slug: "embeds" },
  {
    name: "Template Export / Import",
    component: TemplateExport,
    slug: "template-export-import",
  },
  { name: "Delete", component: Delete, slug: "delete" },
];

onMounted(async () => {
  await store.getBlocks();
});

const tabChanged = (index: any) => {
  const item = navigation[index];
  location.hash = item.slug;
  selectedTabIndex.value = index;
};

const foundIndex = navigation.findIndex(
  (item) => item.slug === location.hash.replace("#", "")
);

if (foundIndex !== -1) {
  tabChanged(foundIndex);
} else {
  tabChanged(0);
}

onUnmounted(() => {
  store.clearForm();
});

store.$patch({
  form: props.form,
});
</script>
