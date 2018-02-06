<template lang="pug">
  main#order
    //- h1 訂單頁面
    section.brand-banner
    section.brand-menu
      .container
        .columns.is-tablet.is-multiline.is-gapless
          .column.is-4-tablet(v-for='(series, index) in teaList', :key='index')
            .series-block
              .series-head
                h4 {{series.seriesName}}
              .series-body
                ul
                  li(v-for='(product, index) in series.list', 
                    :key='index',
                    @click='orderThis(product)',
                    :class='product.isSelected ? "active" : ""')
                    p.product-name {{product.productName}}
                    .product-price
                      span 
                        small M 
                        | {{product.priceM}}
                      span 
                        small L 
                        | {{product.priceL}}
    
    form.order-panel(:class='orderPanelActive ? "active" : ""')
      .order-panel-head(@click='orderPanelActive = !orderPanelActive')
        h4 我要喝
      .order-panel-body
        .product-ordered
          strong {{ordering.name}}
          .cup-size-select
            template(v-for='size in optionsList.size')
              input(:id='`cup-size-${size}`', :value='size', type="radio", name="cupSize", v-model='ordering.size').label-input
              label(:for='`cup-size-${size}`', @click='showPrice') {{size}}
            //- input#cup-size-l(type="radio", name="cupSize").label-input
            //- label(for='cup-size-l') L
        .product-options    
          .option.sugar
            span 甜度
            .sugar-select.options-select
              template(v-for='sugar in optionsList.sugar')
                input(:id='`sugar-${sugar.nameEn}`', :value='sugar.nameTw', type="radio", name="sugar", v-model='ordering.sugar').label-input
                label(:for='`sugar-${sugar.nameEn}`') {{sugar.nameTw}}
              //- input#sugar-regular(type="radio", name="sugar").label-input
              //- label(for="sugar-regular") 正常
              //- input#sugar-less(type="radio", name="sugar").label-input
              //- label(for="sugar-less") 少糖
              //- input#sugar-half(type="radio", name="sugar").label-input
              //- label(for="sugar-half") 半糖
              //- input#sugar-quarter(type="radio", name="sugar").label-input
              //- label(for="sugar-quarter") 微糖
              //- input#sugar-free(type="radio", name="sugar").label-input
              //- label(for="sugar-free") 無糖

          .option.sugar
            span 冰塊
            .ice-select.options-select
              template(v-for='ice in optionsList.ice')
                input(:id='`ice-${ice.nameEn}`', :value='ice.nameTw', type="radio", name="ice", v-model='ordering.ice').label-input
                label(:for='`ice-${ice.nameEn}`') {{ice.nameTw}}
              //- input#ice-regular(type="radio", name="ice", checked).label-input
              //- label(for="ice-regular") 正常
              //- input#ice-less(type="radio", name="ice").label-input
              //- label(for="ice-less") 少冰
              //- input#ice-easy(type="radio", name="ice").label-input
              //- label(for="ice-easy") 微冰
              //- input#ice-free(type="radio", name="ice").label-input
              //- label(for="ice-free") 去冰
              //- input#ice-room(type="radio", name="ice").label-input
              //- label(for="ice-room") 常溫
              //- input#ice-hot(type="radio", name="ice").label-input
              //- label(for="sugar-hot") 熱飲
      
        .sent-order
          .order-owner
            span 我是
            input(type="text", name="orderOwner", v-model='ordering.owner', required)
          .order-submit
            a.button.is-small(:disabled='ordering.name === ""') {{ ordering.name === '' ? '先選啦幹' : `送出但記得要付 $ ${finalPrice}` }}


</template>

<script>
import api from 'axios'

export default {
  data () {
    return {
      teaList: [],
      optionsList: [],
      selectedItem: '',
      ordering: {
        name: '',
        size: 'M',
        price: 0,
        sugar: '正常',
        ice: '正常',
        owner: ''
      },
      orderPanelActive: false
    }
  },
  methods: {
    async getTeaApi () {
      api.get('/static/50.json')
        .then(res => {
          this.teaList = res.data
          this.teaList.forEach(series => {
            series.list.forEach(item => {
              this.$set(item, 'isSelected', false)
            })
          })
        })
    },
    async getOptionsApi () {
      api.get('/static/order-options.json')
        .then(res => {
          this.optionsList = res.data
        })
    },
    orderThis (item) {
      this.teaList.forEach(series => {
        series.list.forEach(obj => {obj.isSelected = false})
      })
      item.isSelected = true
      this.selectedItem = item
      this.orderPanelActive = true
      this.ordering.name = item.productName
    },
    showPrice () {
      if (this.ordering.size === 'M') {
        this.ordering.price = this.selectedItem.priceM
      } else {
        this.ordering.price = this.selectedItem.priceL
      }
    }
  },
  computed: {
    finalPrice () {
      if (this.ordering.size === 'M') {
        return this.ordering.price = this.selectedItem.priceM
      } else {
        return this.ordering.price = this.selectedItem.priceL
      }
    }
  },
  mounted () {
    this.getTeaApi().then(res => this.getOptionsApi())
  }
}
</script>
