<!-- noodle開始 -->
<template>
  <Loading v-model:active="isLoading"
          :can-cancel="true"
          :color="color"
          :on-cancel="onCancel"
          :loader="loader"
          :is-full-page="fullPage"/>
  <div class="tasty pb-36">
    <section class="banner banner-products">
      <div class="banner-title h-100">
        <div class="row  h-100 position-relative g-0">
          <div class="col-md-6 col-12 offset-md-3 banner-title position-absolute">
            <h2 class="text-light"><span class="banner-delicious-title">美味</span>專區</h2>
            <p class="text-light banner-subtitle">DELICIOUS ZONE</p>
          </div>
          <div class="col-md-6 col-12  offset-md-3 banner-info position-absolute">
            <!--pc-->
            <p class="text-light d-none d-md-block">採用天然新鮮食材精心製作，呈現美味。</p>
            <!--mobile-->
            <p class="text-light offset-md-5 d-block d-md-none banner-info-mobile fs-6">採用天然新鮮食材精心製作，<br>呈現美味。</p>
          </div>
        </div>
      </div>
    </section>
  <div>
    <div class="container">
      <div>
        <section class="tasty-menu text-center my-md-100 mt-5" data-aos="fade-right">
          <div class="row text-light justify-evenly justify-content-sm-center ps-md-4">
            <div class="col-lg-2 col-4"><div class="tasty-menu-item border rounded-circle mx-auto mx-lg-3 my-2 d-flex justify-content-center align-items-center" @click="filteredItems('')" :class="{ 'active': selectedItem === '全部商品' }"><div>全部商品</div></div></div>
            <div class="col-lg-2 col-4"><div class="tasty-menu-item border rounded-circle mx-auto mx-lg-3 my-2" @click="filteredItems('湯麵')" :class="{ 'active': selectedItem === '湯麵' }"><div><img src="@/assets/images/chinese-food.png" alt="湯麵"><span class="d-block">湯麵</span></div></div></div>
            <div class="col-lg-2 col-4"><div class="tasty-menu-item border rounded-circle mx-auto mx-lg-3 my-2" @click="filteredItems('涼麵')" :class="{ 'active': selectedItem === '涼麵' }"><div><img src="@/assets/images/noodles2.png" alt="涼麵"><span class="d-block">涼麵</span></div></div></div>
            <div class="col-lg-2 col-4"><div class="tasty-menu-item border rounded-circle mx-auto mx-lg-3 my-2" @click="filteredItems('炸物')" :class="{ 'active': selectedItem === '炸物' }"><div><img src="@/assets/images/fried-chicken.png" alt="炸物"><span class="d-block">炸物</span></div></div></div>
            <div class="col-lg-2 col-4"><div class="tasty-menu-item border rounded-circle mx-auto mx-lg-3 my-2" @click="filteredItems('精緻小菜')" :class="{ 'active': selectedItem === '精緻小菜' }"><div><img src="@/assets/images/skewer.png" alt="精緻小菜"><span class="d-block">小菜</span></div></div></div>
            <div class="col-lg-2 col-4"><div class="tasty-menu-item border rounded-circle mx-auto mx-lg-3 my-2" @click="filteredItems('湯品')" :class="{ 'active': selectedItem === '湯品' }"><div><img src="@/assets/images/soup.png" alt="湯品"><span class="d-block">湯品</span></div></div></div>
          </div>
        </section>
        <section class="tasty-list">
            <div class="d-flex justify-content-end mt-7">
                <input type="text" class="form-control form-search border-2 border-secondary" name="keyword" placeholder="搜尋產品"  ref="serch" v-on:keydown.enter.prevent="handleMouseDown" style="width: 20%;">
                <button type="button" class="btn"><i class="bi bi-search" @click="searchProduct"></i></button>
            </div>
              <div class="filter-product my-5" ref="filter-product" v-for="(catagoryProducts,catagory) in combiProducts" :key="catagory">
                <div class="d-flex justify-content-center">
                  <h2 class="tasty-tite fs-3 border-3 border-start border-dark px-2 text-center">{{ catagory }} <span class="fs-6">{{ title_map[catagory] }}</span></h2>
                </div>
                <div class="row" >
                  <div class="col-12 col-md-6 col-xl-4 tasty-item-block position-relative" v-for= "product in catagoryProducts" :key="product.id" >
                    <div class="heart p-2 position-absolute"><i :class="heartClass(product.id)" @click="toggleFavorite(product.id)"></i></div>
                    <router-link :to="`/product/${ product.id }`" class="card mx-2 my-4 overflow-hidden">
                      <div class="slide-hover">
                        <img :src="product.imageUrl" alt="商品" class="slide-img">
                      </div>
                      <div class="card-body p-3">
                      <div class="mb-4 tasty-item">
                        <div class="d-flex justify-content-between mb-2">
                          <h5 class="card-title fw-bolder tasty-item-title d-block text-dark">{{ product.title }}</h5>
                        </div>
                        <div class="d-flex justify-content-between align-items-center">
                          <div class="card-text  text-md end tasty-item-price text-info fs-6">特價NT$&nbsp;{{ product.price }}</div>
                          <div class="card-text text-dark text-md end fs-7 text-subPrice">&nbsp;<span class="text-decoration-line-through"> NT$ {{ product.origin_price }}</span></div>
                        </div>
                      </div>
                      <div class="d-flex justify-content-between mb-1  tasty-button">
                        <button type="button" class="d-md-block btn btn-dark text-light w-100">加入購物車</button>
                      </div>
                      </div>
                    </router-link>
                    <button type="button" class="card-add-cart d-md-block btn btn-dark text-light position-absolute" @click="addToCart(product.id,qty)">加入購物車</button>
                  </div>
                </div>
              </div>
        </section>
      </div>
      <section class="tasty-menu-fixed" data-aos="fade-right" :class="{ 'show': isMenuFixed }" @click="scrollMeTo">
        <div class="column text-light">
          <div class="d-block bg-info d-flex justify-content-center align-items-center rounded-2" style="width: 40px;height: 40px;"><i class="bi bi-arrow-up" style="color: white;"></i></div>
        </div>
      </section>
    </div>
  </div>
  </div>
</template>

<script>
// loading
import Loading from 'vue-loading-overlay'
import 'vue-loading-overlay/dist/css/index.css'
import { mapActions } from 'pinia'
import cartStore from '@/store/cartStore.js'
const { VITE_APP_URL, VITE_APP_PATH } = import.meta.env
export default {
  data () {
    return {
      products: [],
      title_map:
      {
        涼麵: 'COLD NOODLE',
        湯麵: 'SOUP NOODLE',
        炸物: 'Fried',
        精緻小菜: 'SIDE DESH',
        湯品: 'SOUP'
      },
      combiProducts: {},
      temProducts: [],
      searchProducts: [],
      isfilter: true,
      selectedItem: '',
      favorites: [],
      lodingItem: '',
      isMenuFixed: false,
      qty: 1,
      // loding css
      color: '#9cd020',
      isLoading: false,
      fullPage: true,
      loader: 'bars'
    }
  },
  components: {
    Loading
  },
  methods: {
    testItems (category) {
      this.$http.get(`${VITE_APP_URL}/v2/api/${VITE_APP_PATH}/products/all`)
        .then((res) => {
          this.isfilter = false
          this.products = res.data.products
          this.temProducts = res.data.products
          this.searchProducts = res.data.products
          this.isLoading = false
          this.products = this.products.filter(item => item.category !== '熱門')
          this.combiProducts = {}
          this.products.forEach((item) => {
            if (!this.combiProducts[item.category]) {
              this.combiProducts[item.category] = [] // 初始化
            }
            this.combiProducts[item.category].push(item) // 將 item 推入相應的数組
          })
        })
    },
    filteredItems (categoryName) {
      // 沒有選取分類時取出所有分類產品
      this.combiProducts = {}

      if (categoryName === '') {
        this.products.forEach((item) => {
          if (!this.combiProducts[item.category]) {
            this.combiProducts[item.category] = [] // 初始化
          }
          this.combiProducts[item.category].push(item) // 將 item 推入相应的数组
        })
        return
      }
      // 有選取分類時, 清空combiProducts重取出對應的分類產品
      this.products.forEach((item) => {
        if (categoryName === item.category) {
          if (!this.combiProducts[item.category]) {
            this.combiProducts[item.category] = [] // 初始化
          }
          this.combiProducts[item.category].push(item) // 將 item 推入相應的数組
        }
      })
    },
    // 搜尋篩選
    handleMouseDown () {
      if (this.$refs.serch.value === '') {
        return
      }
      this.searchFunction()
    },

    searchProduct () {
      if (this.$refs.serch.value === '') {
        return
      }
      this.searchFunction()
    },

    searchFunction () {
      this.combiProducts = {}
      this.products.forEach((item) => {
        if (item.title.indexOf(this.$refs.serch.value) !== -1) {
          if (!this.combiProducts[item.category]) {
            this.combiProducts[item.category] = [] // 初始化
          }
          this.combiProducts[item.category].push(item) // 將 item 推入相應的数組
        }
      })
    },

    doAjax () {
      this.isLoading = true
    },
    // localsStorage
    toggleFavorite (productId) {
      if (this.favorites.includes(productId)) {
        this.favorites = this.favorites.filter(id => id !== productId)
      } else {
        this.favorites.push(productId)
      }
      this.heartClass(productId)
      localStorage.setItem('favorites', JSON.stringify(this.favorites))
    },

    heartClass (productId) {
      return this.favorites.includes(productId) ? 'fa-solid fa-heart fs-5 text-danger' : 'fa-regular fs-5 fa-heart'
    },
    setMenuFixed () {
      if (window.scrollY > 500) {
        this.isMenuFixed = true
      } else {
        this.isMenuFixed = false
      }
    },
    scrollMeTo () {
      const top = window.offsetTop - 120

      window.scrollTo(0, top)
    },
    ...mapActions(cartStore, ['addToCart'])
  },
  mounted () {
    this.doAjax()
    this.testItems('category')
    window.addEventListener('scroll', this.setMenuFixed)
    this.favorites = JSON.parse(localStorage.getItem('favorites')) || []
    this.heartClass()
  },
  beforeUnmount () {
    window.removeEventListener('scroll', this.setMenuFixed)
  }
}
</script>
