<template>
  <app-layout title="Dashboard">
    <div class="w-full py-12">
      <div
        class="mx-auto grid max-w-7xl grid-cols-12 gap-x-10 px-4 sm:px-6 lg:px-8"
      >
        <div class="col-span-12 lg:col-span-8">
          <div class="relative mb-6 flex items-center justify-between">
            <h2 class="text-xl font-bold leading-8 text-grey-900">
              Your Forms
            </h2>
            <CreateFormButton />
          </div>

          <div
            v-if="!forms.length || forms.length === 0"
            class="w-full rounded bg-white px-16 py-16 text-center"
          >
            <h2
              class="font-heading mb-4 text-center text-2xl font-black leading-none text-grey-900"
            >
              Start by creating a form
            </h2>
            <p class="text-sm text-grey-600">
              You haven't created a survey yet. Click on "New survey" in the top
              right corner to begin with a new survey. To get you started really
              quickly you can watch the following introduction for the BotReach
              editor.
            </p>
          </div>

          <div v-else>
            <FormListItem :form="form" :key="form.id" v-for="form in forms" />
          </div>
        </div>
        <UpdatesContainer class="col-span-12 mt-6 lg:col-span-4 lg:mt-0" />
      </div>
    </div>
  </app-layout>
</template>

<script lang="ts" setup>
import AppLayout from "@/Layouts/AppLayout.vue";
import FormListItem from "@/components/Dashboard/FormListItem.vue";
import UpdatesContainer from "@/components/Dashboard/UpdatesContainer.vue";
import CreateFormButton from "@/components/Dashboard/CreateFormButton.vue";
import { withDefaults } from "vue";

withDefaults(
  defineProps<{
    forms: Array<FormModel>;
  }>(),
  {
    forms: () => [],
  }
);
</script>
