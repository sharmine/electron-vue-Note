<template>
  <div class="catalog">
    <ul>
      <sd-menu v-for="item in topMenuList" :key="item.uuid" :propItem="item"></sd-menu>
    </ul>
  </div>
</template>

<script type="text/ecmascript-6">
  import sdMenu from '../base/menu.vue'
  import cataLogOperator from '../../business/cataLogOperatore'
  import {cataLogType} from '../../model/enumtype'

  export default {
    components: {
      sdMenu
    },
    data () {
      return {
        topMenuList: [
          {
            uuid: 'topNoteBook',
            name: '笔记本',
            childCount: 1,
            cataLogChain: '',
            childList: [],
            type: cataLogType.personalNote
          }
        ]
      }
    },
    mounted () {
      this.$nextTick(() => {
        this.topMenuList.forEach(item => {
          cataLogOperator.getOneCataLogAllChianInfo(item.uuid, item).then(childList => {
            console.log(childList)
            item.childList = childList
          }).catch(err => {
            this.$alert({
              type: 'error',
              message: err
            })
          })
        })
      })
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss" scoped>
  .catalog{
    width: 280px;
    float: left;
    height: calc(100% - 10px);
    padding: 5px 0px;
    font-size: 18px;
    user-select: none;
    color: #ffffff;
    background-color: #2A333C;
  }
</style>
