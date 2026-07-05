<template>
  <div>
    <h2 class="mb-4">{{ $t('plugin_pathfix.title') }}</h2>

    <div style="margin-bottom: 80px">
      <v-card class="mb-4 pa-0">
        <v-card-text>
          <p class="text-medium-emphasis mb-4">{{ $t('plugin_pathfix.description') }}</p>

          <v-btn color="primary" @click="run" :loading="running">
            <v-icon start>mdi-play</v-icon>
            {{ $t('plugin_pathfix.run') }}
          </v-btn>

          <v-alert
            v-if="result"
            :type="result.success ? 'success' : 'error'"
            variant="tonal"
            class="mt-4"
          >
            {{ result.success ? $t('plugin_pathfix.success') : $t('plugin_pathfix.error') }}
            <pre v-if="result.output" class="mt-2" style="white-space: pre-wrap">{{ result.output }}</pre>
          </v-alert>
        </v-card-text>
      </v-card>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const running = ref(false);
const result = ref(null);

const getAuthHeaders = () => ({
  Authorization: 'Bearer ' + localStorage.getItem('authToken'),
});

const run = async () => {
  running.value = true;
  result.value = null;
  try {
    const res = await fetch('/api/v1/mos/plugins/query', {
      method: 'POST',
      headers: {
        ...getAuthHeaders(),
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        command: 'pathfix',
        args: [],
        timeout: 60,
        parse_json: false,
      }),
    });
    const data = await res.json();
    result.value = {
      success: res.ok && data.success !== false,
      output: (data.output || data.error || '').trim(),
    };
  } catch (e) {
    result.value = { success: false, output: String(e) };
  } finally {
    running.value = false;
  }
};
</script>
