<template>
  <transition :css="false" @enter="enter" @leave="leave">
    <div class="ui segment transition hidden">
      <div class="ui feed">
        <div class="event">
          <div class="label photo-container">
            <img :src="profile.photo">
          </div>
          <div class="content">
            <div class="summary">
              <router-link :to="`/user/${post.owner}`" class="user">
                {{ profile.name }}
              </router-link>
              <div class="date">{{ post.timestamp | fromNow }}</div>
            </div>
            <div class="summary" v-html="post.content">
            </div>
            <div class="meta">
              <a class="like" ref="likeBtn" @click="like">
                <i class="like icon"></i> {{ likeCount }} Likes
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import { PostService } from './../../services'

export default {
  props: ['post', 'profile'],
  methods: {
    enter (el, done) {
      $(el).transition('fade down')
      done()
    },
    leave (el, done) {
      $(el).transition('fade down')
      done()
    },
    like () {
      PostService.like(this.post)
    },
    updateState () {
      const $btn = $(this.$refs.likeBtn)
      if (this.post.isLike) {
        if (!$btn.hasClass('active')) {
          $btn.addClass('active')
          $('.icon', $btn).transition('jiggle')
        }
      } else {
        if ($btn.hasClass('active')) {
          $btn.removeClass('active')
          $('.icon', $btn).transition('pulse')
        }
      }
    }
  },
  computed: {
    likeCount () {
      !this.$refs.likeBtn ? this.$nextTick(this.updateState) : this.updateState()
      return this.post.likes
    }
  }
}
</script>

<style scoped lang="scss">
  .ui.feed > .event > .label {
    width: 3.5em;
    display: flex;
    justify-content: center;
    align-items: center;

    > img {
      flex: none;
    }
  }
</style>