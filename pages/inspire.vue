<template>
  <v-row justify="center" align="center">
    <v-col class="text-center">
      <blockquote class="blockquote">
        &#8220;First, solve the problem. Then, write the code.&#8221;
        <footer>
          <small>
            <em>&mdash;John Johnson</em>
          </small>
        </footer>
      </blockquote>
    </v-col>
    <v-col cols="12">
      <v-card class="py-4 d-flex justify-center">
        <mock-ajax page="/inspire" />
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import mockAjax from '~/components/mock-ajax.vue'
export default {
  components: { mockAjax },
  name: 'InspirePage',
}
</script>
