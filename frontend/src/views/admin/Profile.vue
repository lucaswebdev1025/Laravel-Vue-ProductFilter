<template>
  <admin-dashboard-layout>
    <v-container fluid class="fill-height p-5">
      <v-row>
        <v-col>
          <v-card class="mx-auto my-auto">
            <v-card-text>
              <v-row>
                <v-col>
                  <avatar-edit
                    :current_url="user.avatar_url"
                    @input="handleUpdateProfile"
                    v-model="user.avatar_url"
                  ></avatar-edit>
                </v-col>
                <v-col>
                  <v-form>
                    <v-container>
                      <v-row>
                        <v-col>
                          <v-text-field
                            v-model="user.name"
                            label="Name"
                            placeholder=""
                          ></v-text-field>
                        </v-col>
                      </v-row>
                      <v-row>
                        <v-col>
                          <v-btn @click="handleUpdateProfile"> Update</v-btn>
                        </v-col>
                      </v-row>
                    </v-container>
                  </v-form>
                </v-col>
              </v-row>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </admin-dashboard-layout>
</template>

<script>
import User from "../../models/User";
import AvatarEdit from "../../components/AvatarEdit";
import AdminDashboardLayout from "../../layouts/AdminDashboardLayout";
export default {
  name: "Profile",
  components: { AdminDashboardLayout, AvatarEdit },
  data: function () {
    return {
      user: {},
    };
  },
  methods: {
    async handleUpdateProfile() {
      let userData = this.$auth.user();
      let user = new User(userData);
      this.user = await user.save();
    },
  },
  mounted() {
    this.user = this.$auth.user();
  },
};
</script>

<style scoped>
</style>
