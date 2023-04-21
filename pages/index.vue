<template>
  <v-row justify="center" align="center">
    <v-col cols="12">
      <v-card class="py-4 d-flex justify-center">
        <mock-ajax page="/" />
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import mockAjax from '~/components/mock-ajax.vue'
export default {
  components: { mockAjax },
  name: 'IndexPage',
}
</script>
