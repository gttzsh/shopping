<template>
  <div class="prolist">
    <ul v-for="item in prolist">
      <li :key="item.id">
        {{item.name}}-
        <span>{{item.praice}}元</span>
        <a
          href="#"
          class="shopping"
          @click="addHandle(item.id, $event)"
        >加入购物车
        </a>
      </li>
    </ul>
  </div>
</template>

<script>
import event from '../event'
export default {
  props: {
    prolist: {
      type: Array,
      default() {
        return []
      }
    }
  },
  methods: {
    addHandle(id, e) {
      e.preventDefault()
      event.$emit('addCarList', id)
    }
  }
}
</script>
