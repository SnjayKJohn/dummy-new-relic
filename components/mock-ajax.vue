<template>
  <v-card width="1000" >
    <v-card-title> Mock Ajax from {{ page }}</v-card-title>
    <v-card-text>
      <v-expansion-panels v-model="panel" variant="popout" class="my-4">
        <v-expansion-panel
          v-for="(item, i) in list"
          :key="i"
          title="Item"
          text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."
        >
          <v-expansion-panel-header
            >{{ toUpperCase(item.type) }} /{{
              item.url
            }}</v-expansion-panel-header
          >
          <v-expansion-panel-content>
            <v-card>
              <v-card-text> Payload : {{ item.payload }} </v-card-text>
              <v-card-actions>
                <v-spacer />
                <v-btn
                  color="primary"
                  @click="triggerAjax(item)"
                  :loading="loader"
                  >Trigger</v-btn
                >
              </v-card-actions>
            </v-card>
          </v-expansion-panel-content>
        </v-expansion-panel>
      </v-expansion-panels>
    </v-card-text>

    <v-dialog v-model="dialog" max-width="1200">
      <v-card>
        <v-card-title class="text-h5"> Response </v-card-title>
        <v-card-text>{{ response }}</v-card-text>
        <v-card-actions class="justify-end">
          <v-btn text @click="dialog = false">Close</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-card>
</template>

<script>
export default {
  props: {
    page: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      panel: null,
      loader: false,
      dialog: false,
      response: '',
      list: [
        {
          url: 'users/account/setting',
          type: 'get',
          payload: {},
        },
        {
          url: 'shipments/status',
          type: 'get',
          payload: {
            params: { tab: 'drafts' },
          },
        },
        {
          url: 'shipments',
          type: 'get',
          payload: {
            params: {
              status: 'in_progress',
              'sort[created]': 'desc',
              importProgressStatus: ['checkout', 'buy_in_progress'],
              outputPropertyFilter: ['uuid'],
            },
          },
        },
      ],
    }
  },
  methods: {
    toUpperCase(text) {
      return text.toUpperCase()
    },
    async triggerAjax(item) {
      this.loader = true
      try {
        const { data } = await this.$nuxt.$axios[item.type](
          item.url,
          item.payload
        )
        this.response = data
      } catch (error) {
        this.response = error
      } finally {
        this.loader = false
      }
      this.dialog = true
    },
  },
}
</script>

<style></style>
