<template>
  <div>
    <p class="text-3xl primary--text font-weight-semibold">
      作者详情
    </p>
    <v-row>
      <v-col
        cols="12"
        md="9"
      >
        <v-card>
          <div class="d-flex flex-no-wrap justify-space-between">
            <div
              v-if="authorDetail.avatar!=='https://song.gushiwen.cn/'&&$vuetify.breakpoint.mdAndUp"
              class="mt-6 ml-6 mb-6"
            >
              <v-img
                :src="authorDetail.avatar"
                class="rounded-lg"
                width="88px"
                height="100%"
              ></v-img>
            </div>
            <div class="mt-3">
              <v-list-item>
                <v-list-item-avatar
                  v-if="authorDetail.avatar!=='https://song.gushiwen.cn/'&&!$vuetify.breakpoint.mdAndUp"
                  size="35"
                >
                  <v-img
                    :src="authorDetail.avatar"
                  ></v-img>
                </v-list-item-avatar>

                <v-list-item-content>
                  <v-list-item-title class="font-weight-bold text-xl">
                    {{ authorDetail.name }}
                  </v-list-item-title>
                </v-list-item-content>
              </v-list-item>
              <v-card-text>
                <div
                  class="text--primary"
                >
                  <span class="font-weight-regular">
                    {{ authorDetail.lifetime }}
                  </span>
                </div>
              </v-card-text>
            </div>
          </div>
        </v-card>
      </v-col>

      <v-col
        v-for="data in authorDetail.describe"
        :key="data.id"
        cols="12"
        md="9"
      >
        <v-card class="text--primary">
          <v-card-title>
            <h3>{{ data.type }}</h3>
          </v-card-title>
          <div
            v-for="(content,index) in data.content"
            :key="index"
          >
            <v-card-text v-if="setTitle(content)">
              <h3>{{ content }}</h3>
            </v-card-text>
            <v-card-text
              v-else
              class="author-detail-content"
            >
              {{ content }}
            </v-card-text>
          </div>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>
<script>
import { getAuthorByID } from '@/api/author'

export default {
  data() {
    return {
      authorDetail: {},
    }
  },
  mounted() {
    this.getAuthor()
  },
  methods: {
    setTitle(content) {
      const reg = RegExp(/[《》，。、]/)
      if (content.length >= 7 || reg.test(content)) {
        return false
      }

      return true
    },
    getAuthor() {
      getAuthorByID(this.$route.params.id)
        .then(res => {
          this.authorDetail = res.data

          // 对读取的数据进行筛选
          this.authorDetail.avatar = `https://song.gushiwen.cn/${this.authorDetail.avatar}`
          this.authorDetail.describe = this.authorDetail.describe.filter(data => data.content !== null)
        })
    },
  },
}
</script>
<style>
.author-detail-content {
  text-indent:2em;
  letter-spacing: 0.4px;
}
</style>
