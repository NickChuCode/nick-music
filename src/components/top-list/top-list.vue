<template>
  <transition name="slide">
    <music-list :rank="rank" :title="title" :bg-image="bgImage" :songs="songs"></music-list>
  </transition>
</template>

<script>
import MusicList from 'components/music-list/music-list'
import { getMusicList } from 'api/rank'
import { mapGetters } from 'vuex'
import { createSong, isValidMusic, processSongsUrl } from 'common/js/song'
import {ERR_OK} from 'api/config'
export default {
  name: 'top-list',
  components: {
    MusicList
  },
  data() {
    return {
      songs: [],
      rank: true
    }
  },
  computed: {
    title() {
      return this.toplist.topTitle
    },
    bgImage() {
      return this.toplist.picUrl
    },
    ...mapGetters([
      'toplist'
    ])
  },
  created() {
    this._getMusicList()
  },
  methods: {
    _getMusicList() {
      if (!this.toplist.id) {
        this.$router.push({
          path: '/rank'
        })
      }
      getMusicList(this.toplist.id).then((res) => {
        if (res.code === ERR_OK) {
          console.log(res)
          processSongsUrl(this._normalizeSinger(res.songlist)).then((songs) => {
            this.songs = songs
          })
        }
      })
    },
    _normalizeSinger(list) {
      let ret = []
      list.forEach((item) => {
        let musicData = item.data
        if (isValidMusic(musicData)) {
          ret.push(createSong(musicData))
        }
      })
      return ret
    }
  }
}
</script>

<style scoped lang="stylus">
  .slide-enter-active, .slide-leave-active {
    transition all 0.3s ease
  }
  .slide-enter, .slide-leave-to {
    transform translate3d(100%, 0, 0)
  }
</style>
