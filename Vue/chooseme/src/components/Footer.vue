<template>
  <div>
    <v-card height="150px">
      <v-footer
        padless="padless"
      >
        <v-card
          flat
          tile
          width="100%"
          class="grey darken-4 text-center"
        >
          <v-card-text>
            <v-btn
              v-for="icon in icons"
              :key="icon"
              class="mx-4"
              icon
            >
              <v-icon color="white" size="24px">
                {{ icon }}
              </v-icon>
            </v-btn>
          </v-card-text>

          <v-divider></v-divider>

          <v-card-text class="white--text">
            {{ new Date().getFullYear() }} — <strong>ChooseMe</strong>
          </v-card-text>
        </v-card>
      </v-footer>
    </v-card>
  </div>
</template>

<script>
  export default {
    data: () => ({
      icons: [
        'mdi-home',
        'mdi-email',
        'mdi-calendar',
        'mdi-delete',
      ],
      items: [
        'default',
        'absolute',
        'fixed',
      ],
      padless: true,
      variant: 'absolute',
    })
  }
</script>