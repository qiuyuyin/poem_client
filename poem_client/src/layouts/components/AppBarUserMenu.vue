<template>
  <v-menu
    offset-y
    left
    nudge-bottom="14"
    min-width="230"
    content-class="user-profile-menu-content"
    class=""
  >
    <template
      v-slot:activator="{ on, attrs }"
    >
      <v-badge
        v-if="$store.state.user.token !== ''"
        bottom
        color="success"
        overlap
        offset-x="12"
        offset-y="12"
        class="ms-4"
        dot
      >
        <v-avatar
          size="40px"
          v-bind="attrs"
          v-on="on"
        >
          <template v-if="$store.state.user.userInfo.headerImg === ''">
            <img
              :src="avater"
            >
          </template>
          <template v-else>
            <v-img
              :src="$store.state.user.userInfo.headerImg"
            >
            </v-img>
          </template>
        </v-avatar>
      </v-badge>
      <v-btn
        v-if="$store.state.user.token === ''"
        width="40px"
        small
        class="primary--text ml-2"
        :to="{name:'pages-login'}"
      >
        登录
      </v-btn>
    </template>
    <v-list v-if="$store.state.user.token !== ''">
      <div class="pb-3 pt-2">
        <v-badge
          bottom
          color="success"
          overlap
          offset-x="12"
          offset-y="12"
          class="ms-4"
          dot
          @click="()=>{dialog = true}"
        >
          <v-avatar size="40px">
            <template v-if="$store.state.user.userInfo.headerImg === ''">
              <img
                :src="avater"
              >
            </template>
            <template v-else>
              <v-img
                :src="$store.state.user.userInfo.headerImg"
              >
              </v-img>
            </template>
          </v-avatar>
        </v-badge>
        <div
          class="d-inline-flex flex-column justify-center ms-3"
          style="vertical-align:middle"
        >
          <span class="text--primary font-weight-semibold mb-n1">
            {{ $store.state.user.userInfo.nickName }}
          </span>
          <small class="text--disabled text-capitalize">{{ $store.state.user.userInfo.userName }}</small>
        </div>

        <v-dialog
          v-model="dialog"
          width="500"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              class="ma-2"
              text
              v-bind="attrs"
              color="primary"
              v-on="on"
            >
              修改头像
            </v-btn>
          </template>
          <v-card>
            <account-settings-account>
            </account-settings-account>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="primary"
                text
                @click="dialog = false"
              >
                退出
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </div>

      <!-- <v-divider></v-divider> -->

      <!-- Profile -->
      <!-- <v-list-item
        link
      >
        <v-list-item-icon class="me-2">
          <v-icon size="22">
            {{ icons.mdiAccountOutline }}
          </v-icon>
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title>用户信息</v-list-item-title>
        </v-list-item-content>
      </v-list-item> -->

      <!-- Email
      <v-list-item link>
        <v-list-item-icon class="me-2">
          <v-icon size="22">
            {{ icons.mdiEmailOutline }}
          </v-icon>
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title>Inbox</v-list-item-title>
        </v-list-item-content>
      </v-list-item> -->

      <!-- Chat -->
      <!-- <v-list-item link>
        <v-list-item-icon class="me-2">
          <v-icon size="22">
            {{ icons.mdiChatOutline }}
          </v-icon>
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title>聊天</v-list-item-title>
        </v-list-item-content>

        <v-list-item-action>
          <v-badge
            inline
            color="error"
            content="2"
          >
          </v-badge>
        </v-list-item-action>
      </v-list-item> -->

      <!-- <v-divider class="my-2"></v-divider> -->

      <!-- Settings -->
      <!-- <v-list-item
        link
      >
        <v-list-item-icon class="me-2">
          <v-icon size="22">
            {{ icons.mdiCogOutline }}
          </v-icon>
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title>用户设置</v-list-item-title>
        </v-list-item-content>
      </v-list-item> -->

      <!-- Pricing
      <v-list-item link>
        <v-list-item-icon class="me-2">
          <v-icon size="22">
            {{ icons.mdiCurrencyUsd }}
          </v-icon>
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title>Pricing</v-list-item-title>
        </v-list-item-content>
      </v-list-item> -->

      <!-- FAQ -->
      <!-- <v-list-item link>
        <v-list-item-icon class="me-2">
          <v-icon size="22">
            {{ icons.mdiHelpCircleOutline }}
          </v-icon>
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title>关于我们</v-list-item-title>
        </v-list-item-content>
      </v-list-item> -->

      <v-divider class="my-2"></v-divider>

      <!-- Logout -->
      <v-list-item
        @click="LoginOut"
      >
        <v-list-item-icon class="me-2">
          <v-icon size="22">
            {{ icons.mdiLogoutVariant }}
          </v-icon>
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title>登出</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
    </v-list>
  </v-menu>
</template>

<script>
import {
  mdiAccountOutline,
  mdiEmailOutline,
  mdiCheckboxMarkedOutline,
  mdiChatOutline,
  mdiCogOutline,
  mdiCurrencyUsd,
  mdiHelpCircleOutline,
  mdiLogoutVariant,
} from '@mdi/js'
import { mapActions } from 'vuex'
import { generateFromString } from 'generate-avatar'
import AccountSettingsAccount from '@/views/pages/account-settings/AccountSettingsAccount.vue'

export default {
  components: {
    AccountSettingsAccount,
  },
  data() {
    return {
      dialog: false,
    }
  },
  computed: {
    avater() {
      if (this.$store.state.user.userInfo.headerImg !== '') { return this.$store.state.user.userInfo.headerImg }
      const str = generateFromString(this.$store.state.user.userInfo.userName)
      console.log(str)

      return `data:image/svg+xml;utf8,${str}`
    },
  },
  methods: {
    ...mapActions('user', ['LoginOut']),
    getAvater() {
      console.log(this.$store.state.user.userInfo.userName)
      generateFromString(this.$store.state.user.userInfo.username)
    },
  },
  setup() {
    return {
      icons: {
        mdiAccountOutline,
        mdiEmailOutline,
        mdiCheckboxMarkedOutline,
        mdiChatOutline,
        mdiCogOutline,
        mdiCurrencyUsd,
        mdiHelpCircleOutline,
        mdiLogoutVariant,
      },
    }
  },
}
</script>

<style lang="scss">
.user-profile-menu-content {
  .v-list-item {
    min-height: 2.5rem !important;
  }
}
</style>
