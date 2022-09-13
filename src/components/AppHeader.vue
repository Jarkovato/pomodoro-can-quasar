<template>
  <q-header elevated height-hint="150">
    <q-toolbar>
      <q-btn
        flat
        dense
        round
        icon="menu"
        aria-label="Menu"
        @click="this.$emit('toggleLeftDrawer')"
      />
      <q-toolbar-title> Pomodoro can </q-toolbar-title>
      <q-select
        v-model="lang"
        :options="langOptions"
        filled
        borderless
        emit-value
        map-options
        style="width: 140px"
      />
      <q-toggle
        v-model="isDarkThemeEnabled"
        color="white"
        label="Dark theme"
        @update:model-value="toggleDarkTheme"
      />
    </q-toolbar>
  </q-header>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from 'vue';
import { useQuasar } from 'quasar';
import { capitalizeFirstLetter } from './helpers';
import languages from 'quasar/lang/index.json';

const appLanguages = languages.filter((lang) =>
  ['en-US', 'ru'].includes(lang.isoName)
);

const langOptions = appLanguages.map((lang) => ({
  label: capitalizeFirstLetter(lang.nativeName),
  value: lang.isoName,
}));

export default defineComponent({
  name: 'AppHeader',
  setup() {
    const $q = useQuasar();
    const isDarkThemeEnabled = ref(false);
    const lang = ref($q.lang.isoName);

    watch(lang, (val) => {
      import(`quasar/lang/${val}`).then((lang) => {
        $q.lang.set(lang.default);
      });
    });

    return {
      isDarkThemeEnabled,
      lang,
      langOptions,
      toggleDarkTheme(flag: boolean): void {
        $q.dark.set(flag);
      },
    };
  },
});
</script>
