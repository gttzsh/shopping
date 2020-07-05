<template>
  <div id="app">
     <Upload/>
    <div class="shopTable">
      <div class="tab">购物车</div>
      <ProductionsList :prolist="productionList"/>
      <hr/>
      <CarList
        :carlist="carList"
        :prolist="productionList"/>
    </div>
  </div>
</template>

<script>
import ProductionsList from './components/ProductionsList'
import CarList from './components/CarList'
import Upload from './components/upload'
import event from './event'

export default {
  components: {
    ProductionsList,
    CarList,
    Upload
  },
  data() {
    return {
      productionList: [
        {
          id: 1,
          name: '鼠标',
          praice: 20
        },
        {
          id: 2,
          name: '键盘',
          praice: 60
        },
        {
          id: 3,
          name: '电脑',
          praice: 2000
        }
      ],
      carList: [
        {
          id: 1,
          quantity: 1
        }
      ],
      total: 0
    }
  },
  methods: {
    //往购物车加商品
    addCarList(id) {
      const pro = this.carList.find(item => item.id === id)
      if (pro) {
        pro.quantity++
        return
      }
      this.carList.push({
        id,
        quantity: 1
      })
    },
    //减少商品数量
    deleteQuantity(id) {
      const car = this.carList.find(item => item.id ===id)
      if(car == null) {
        return
      }
      car.quantity--
      if(car.quantity <=0) {
        this.carList = this.carList.filter(item => item.id !==id)
      }
      //--------------
      // var index = this.carList.findIndex(item => {
      //   return item.id == id;
      // })
      // this.carList.splice(index,1);
    },
    //增加
    addQuantity(id) {
      const car = this.carList.find(item => item.id ===id)
      if(car){
        car.quantity++
        return
      }

    }
  },
  mounted() {
    event.$on('addCarList', this.addCarList)
    event.$on('deleteQuantity', this.deleteQuantity)
    event.$on('addQuantity', this.addQuantity)

  }
}
</script>
