<template>
  <div class="search-box" bg-white>
  <div class="search-box-wrapper" @click="onSearchclick">
    <l-icon size="30" name="research" class="research"></l-icon>
    <input ref="input" @focus="focus" @confirm="onConfirm" @input="onChange" v-model="query" :placeholder="hotSearch" class="input"/>
    <l-icon v-if="query.length>0" @click.stop="deleteQuery"  size="30" name="delete" class="delete"></l-icon>
     <van-dialog id="van-dialog"></van-dialog>
  </div>
  <div class="search-box-button" v-if="showbtn">
    <l-button width="80" height="50">搜索</l-button>
  </div>
 
  </div>
</template>
<script>
import dialog from 'vant-weapp/dist/dialog/dialog'
export default {
  name: '',
  props: {
    disabled: {
      type: Boolean,
      default: false
    },
    focus: {
      type: Boolean,
      default: false
    },
    limit: {
      type: Number,
      default: 50
    },
    hotSearch: {
      type: String,
      default: 'computer'
    },
    showbtn: Boolean
  },
  data () {
    return {
      query: '',
      hotquery: ''

    }
  },

  components: {},
  created () {},

  computed: {},

  beforeMount () {},

  mounted () {
  },

  methods: {
    onSearchclick () {
      this.$emit('oncatchclick')
    },
    onConfirm (e) {
      this.$emit('confirm', e.mp.detail.value)
    },
    getvalue () {
      return this.query
    },
    setvalue (query) {
      this.query = query
    },
    onChange (e) {
      let value = e.mp.detail.value
      if (value && value.length > this.limit) {
        value = value.slice(0, this.limit)
        this.query = this.query.slice(0, this.limit)
      }
      this.$emit('onChange', value)
    },
    deleteQuery () {
      dialog.confirm({
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        message: '确定要删除吗'
      }).then(() => {
        this.query = ''
        this.$emit('deleted', this.query)
      }).catch(() => {
      })
    },
    addquery (query) {
      this.query = query
    },
    blur () {
      this.$refs.input.blur()
    }

  },
  watch: {}
}
</script>
<style lang='stylus' scoped>
.search-box
  z-index 2
  padding 15px 15px 15px 15px
  display flex
  align-items center
  .search-box-wrapper
    flex 1
    height 40px
    display flex
    justify-content space-between
    align-items center
    background #F5F7F9
    border-radius 20px
    .research
      margin-left 8px
      margin-right 12px
      font-size 15px
    .input
      flex 1
      font-size 14px
      color #333
      height 100%
    .delete
      margin-right 6px
      margin-left 12px
      font-size 15px
  .search-box-button
    height 40px
    height 25px
    margin-left 10px

</style>
