<template >
  <div>
    <ul v-for="item in list">
      <li :key="item.is">
        {{item.name}}数量({{item.quantity}})
        <span class="commond">
          <a href="#" @click="addHander(item.id, $event)">增加</a>
          <a href="#" @click="deleteHander(item.id, $event)">减少</a>
        </span>
      </li>
    </ul>
    <hr/>
    <Total :total="totalPraice"/>
  </div>
</template>
<script>
import event from '../event'
import Total from './Total'
export default {
  components: {
    Total
  },
  props: {
    carlist: {
      type: Array,
      default() {
        return []
      }
    },
    prolist: {
      type: Array,
      default() {
        return []
      }
    }
  },
  computed: {
    //购物车商品列表
    list() {
      return this.carlist.map(listitem => {
        //通过id找到对应的商品信息
        const proListItem = this.prolist.find(proItem => proItem.id === listitem.id)
        //返回商品信息，加购物数量
        return {
          ...proListItem,
          quantity: listitem.quantity
        }
      })
    },
    totalPraice() {
      return this.list.reduce(
        (total, curItem) => total+(curItem.quantity * curItem.praice),0
      )
    }
  },
  methods: {
    deleteHander(id, e) {
      e.preventDefault()
      event.$emit('deleteQuantity', id)
    },
    addHander(id, e) {
      e.preventDefault()
      event.$emit('addQuantity', id)
    }
  }
}
</script>
