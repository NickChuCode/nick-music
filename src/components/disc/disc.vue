<template>
    <transition name="slide">
      <music-list :title="title" :bg-image="bgImage" :songs="songs"></music-list>
    </transition>
</template>

<script>
import MusicList from 'components/music-list/music-list'
import { mapGetters } from 'vuex'
import { getSongList } from 'api/recommend'
import {ERR_OK} from 'api/config'
import { createSong, processSongsUrl } from 'common/js/song'

export default {
  name: 'disc',
  components: {
    MusicList
  },
  data() {
    return {
      songs: []
    }
  },
  computed: {
    title() {
      return this.disc.dissname
    },
    bgImage() {
      return this.disc.imgurl
    },
    ...mapGetters([
      'disc'
    ])
  },
  created() {
    this._getSongList()
  },
  methods: {
    _getSongList() {
      getSongList(this.disc.dissid).then((res) => {
        if (res.code === ERR_OK) {
          processSongsUrl(this._normalizeSongs(res.cdlist[0].songlist)).then((songs) => {
            this.songs = songs
          })
        }
      })
    },
    _normalizeSongs(list) {
      let ret = []
      list.forEach((musicData) => {
        if (musicData.songid && musicData.albumid) {
          ret.push(createSong(musicData))
        }
      })
      return ret
    }
  }
}
</script>

<style scoped lang="stylus">
  .slide-enter-active,.slide-leave-active {
    transition all 0.3s
  }

  .slide-enter,.slide-leave-to {
    transform translate3d(100%, 0, 0)
  }
</style>
