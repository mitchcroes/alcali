<template>
  <v-container>
    <v-card>
      <v-card-title>User Settings</v-card-title>
      <v-card-text>
        <v-container>
          <v-row>
            <v-col lg="2">
              <span>Jobs Notifications</span>
              <div v-for="(val, name) in notifs" :key="name">
                <v-switch v-model="notifs[name]" :label="name" color="primary" hide-details></v-switch>
              </div>
            </v-col>
            <v-col lg="2">
              <span>Max Notifications</span>
              <v-text-field v-model="max_notifs" type="number"></v-text-field>
            </v-col>
          </v-row>
        </v-container>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="primary" @click="updateUserSettings">Submit</v-btn>
      </v-card-actions>
    </v-card>
  </v-container>
</template>

<script>
  export default {
    name: "UserSettings",
    data() {
      return {
        switch1: true,
        switch2: false,
        notifs: { created: false, published: true, returned: false, event: false },
        settings: null,
        max_notifs: null,
      }
    },
    methods: {
      loadData() {
        this.$http.get("api/userssettings/" + this.$store.getters.user_id + "/").then(response => {
          this.settings = response.data
          this.max_notifs = response.data.max_notifs
          Object.keys(this.notifs).forEach(notif => {
            this.notifs[notif] = this.settings["notifs_" + notif]
          })
        })
      },
      updateUserSettings() {
        let params = { max_notifs: this.max_notifs }
        Object.keys(this.notifs).forEach(notif => {
          params["notifs_" + notif] = this.notifs[notif]
        })
        this.$http.patch("api/userssettings/" + this.$store.getters.user_id + "/", params).then(response => {
          this.$toast("user settings updated")
        })
      },
    },
    mounted() {
      this.loadData()
    },
  }
</script>

<style scoped>

</style>