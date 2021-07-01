<template lang="pug">
#list
  #list-content.text-secondary
    b-row
      b-col(cols='12').add.p-4
        b-col(cols='3')
          b-form-group(label='新增事項' invalid-feedback='請至少輸入兩個字' :state='state')
            b-form-input(v-model='newitem' :state='state' trim @keydown.enter='additem')
          b-btn.button(variant='secondary' @click='additem') 新增
      b-col(cols='6').todo.p-5
        h2 待辦事項
        br
        b-table(:items='list' :fields='listfields').text-secondary
          template(#cell(name)='data')
            b-form-input(
              v-if='data.item.edit'
              v-model='data.item.model'
              trim
              :state='data.item.state'
              @keydown.enter='changelist(data.index)'
              @keydown.esc='cancellist(data.index)'
            )
            span#todo-item(v-else) {{ data.value }}
          template(#cell(action)='data')
            span(v-if='!data.item.edit')
              b-btn(@click='editlist(data.index)' variant='success')
                font-awesome-icon(:icon='["fas", "pen"]')
              b-btn(@click='dellist(data.index)' variant='danger')
                font-awesome-icon(:icon='["fas", "trash"]')
            span(v-else)
              b-btn(@click='changelist(data.index)' variant='success')
                font-awesome-icon(:icon='["fas", "check"]')
              b-btn(@click='cancellist(data.index)' variant='danger')
                font-awesome-icon(:icon='["fas", "undo"]')
      #divider
      b-col(cols='6').done.p-5
        h2 已完成事項
        br
        b-table-simple.text-secondary
          thead
            tr
              th 名稱
              th 操作
          tbody
            tr(v-for='(item, idx) in finished' :key='idx')
              td#done-item {{ item }}
              td
                b-btn(@click='delfinish(idx)' variant='danger')
                  font-awesome-icon(:icon='["fas", "trash"]')
</template>

<script>
export default {
  name: 'List',
  data () {
    return {
      newitem: '',
      listfields: [
        { key: 'name', label: '名稱' },
        { key: 'action', label: '操作' }
      ]
    }
  },
  computed: {
    state () {
      if (this.newitem.length === 0) {
        return null
      } else if (this.newitem.length < 2) {
        return false
      } else {
        return true
      }
    },
    list () {
      return this.$store.getters.list.map(item => {
        if (item.model.length < 2) {
          item.state = false
        } else {
          item.state = true
        }
        return item
      })
    },
    finished () {
      return this.$store.state.finished
    }
  },
  methods: {
    additem () {
      if (this.state) {
        this.$store.commit('addList', this.newitem)
        this.newitem = ''
      } else {
        this.$swal({
          icon: 'error',
          title: '錯誤',
          text: '必須要兩個字以上'
        })
      }
    },
    editlist (index) {
      this.$store.commit('editList', index)
    },
    changelist (index) {
      console.log(index)
      if (this.list[index].state) {
        this.$store.commit('changeList', index)
      }
    },
    cancellist (index) {
      this.$store.commit('cancelList', index)
    },
    dellist (index) {
      this.$store.commit('delList', index)
    },
    delfinish (index) {
      this.$store.commit('delFinish', index)
    }
  }
}
</script>
