<template>
  <Loading v-model:active="isLoading"
          :can-cancel="true"
          :color="color"
          :on-cancel="onCancel"
          :loader="loader"
          :is-full-page="fullPage"/>
  <div class="tastyDetail">
    <div>
    <section class="banner banner-product">
      <div class="banner-title h-100">
        <div class="row  h-100 position-relative g-0">
          <div class="col-md-6 col-12  offset-md-3 banner-title position-absolute">
            <h2 class="text-light"><span class="banner-delicious-title">美味</span>專區</h2>
            <p class="text-light banner-subtitle">DELICIOUS ZONE</p>
          </div>
          <div class="col-md-10 col-12 banner-info position-absolute">
            <!--pc-->
            <p class="text-light offset-md-5 d-none d-md-block">採用天然新鮮食材精心製作，呈現美味。</p>
            <!--mobile-->
            <p class="text-light offset-md-5 d-block d-md-none banner-info-mobile fs-6">採用天然新鮮食材精心製作，<br>呈現美味。</p>
          </div>
        </div>
      </div>
    </section>

    <div class="container">
      <section class="tastyDetail-breadcrumb d-flex">
        <div class="tastyDetail-breadcrumb-list d-flex justify-content-between">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb my-2 my-md-4  fs-6">
              <router-link to="/" class="breadcrumb-item text-dark router-link-custom" style="text-decoration:none;">首頁</router-link>
              <router-link to="/products" class="breadcrumb-item text-dark router-link-custom" style="text-decoration: none;">美味專區</router-link>
              <li class="breadcrumb-item active" aria-current="page">產品詳頁</li>
            </ol>
          </nav>
        </div>
      </section>
      <section class="tastyDetail-product">
        <div class="tastyDetail-product-item">
          <div class="row justify-content-center align-items-center">
            <div class="col-12 col-md-6">
              <div><img :src="product.imageUrl" alt="商品" class="rounded"></div>
            </div>
            <div class="product-info col-12 col-md-6 fs-6">
              <div class="row">
                <div class="col-12 mb-md-4 my-4"><h1>{{ product.title }}<span class="badge bg-info ms-2 product_type">{{ product.category }}</span></h1></div>
                <div class="col-lg-3 mb-2">產品描述:</div>
                <div class="col-lg-9 pb-3 mb-2 ps-0 description">{{ product.description }}</div>
                <div class="col-lg-3 mb-2">商品內容:</div>
                <div class="col-lg-9 pb-3 ps-0 content">{{ product.content }}</div>
                <div class="col-3 pb-3 product-price">價格:</div>
                <div class="col-9 pb-3 ps-0 product-price"><span class="price text-info">{{ product.price }}</span>元</div>
              </div>
              <div class="row justify-content-between mt-4">
                <div class="input-group" style="width: 180px;">
                  <button class="btn btn-outline-secondary rounded-0 px-2" type="button" @click.prevent="reproductQty (-1)"><i class="bi bi-dash-lg"></i></button>
                  <input type="number" class="form-control text-center" v-model="product.qty" placeholder="" aria-label="Example text with two button addons readonly">
                  <button class="btn btn-outline-secondary rounded-0 px-2" type="button" @click.prevent="reproductQty (1)"><i class="bi bi-plus"></i></button>
                </div>
                <button type="button" class="col-5 btn btn-dark text-light" @click="addToCart(product.id,product.qty)">加到購物車</button>
              </div>
            </div>
          </div>
        </div>
      </section>
      <section class="customer-notice container-fluid p-4 my-md-8 my-6">
        <div class=" border border-1 border-dark p-4 fs-6">
          <p class="h5 text-center mb-4 fs-5 fw-bold">客戶須知:</p>
          <ol class="ps-md-4">
            <li class="py-2">線上訂購預約時間：如果您透過線上訂購進行當日預約，請預留約20分鐘的時間，我們將為您準備好餐點，以確保順利取餐。</li>
            <li class="py-2">外袋包裝:為了提供良好的使用體驗，我們將麵體和湯類分開包裝，確保麵食的口感和湯汁的風味保持最佳狀態。</li>
            <li class="py-2">新鮮煮熟麵的最佳食用時限：我們建議您盡快食用新鮮煮熟的麵食，以確保最佳的口感和風味。</li>
            <li class="py-2">訂購方式：您可以選擇在店內現場訂購，或透過我們的線上訂購系統進行訂購。</li>
            <li class="py-2">付款方式：為了您的方便，我們接受現金、Line Pay和信用卡三種付款方式。您可以根據個人偏好選擇最適合您的付款方式。</li>
          </ol>
        </div>

      </section>
      <section class="my-0 my-md-4">
        <div class="maybe-like">
          <h3 class="text-center mb-5">或許你會喜歡... </h3>
        </div>
        <div class="swiper-father mx-md-3">
          <swiper :slidesPerView="3" :spaceBetween="30" :autoHeight="true" :autoplay="{delay:3000}" :centeredSlides="false" :loop="true" :navigation="{nextEl:'.swiper-button-next',prevEl:'.swiper-button-prev'}" :modules="modules"  :breakpoints="{0: {slidesPerView: 1,spaceBetween: 10,
      },
      600: {
        slidesPerView: 2,
        spaceBetween: 10,
      },769: {
        slidesPerView: 2,
        spaceBetween: 20,
      },
      1025: {
        slidesPerView: 3,
        spaceBetween: 20,
      },
      }" class="mySwiper" ref="mySwiper">
            <!-- Additional required wrapper -->
            <swiper-slide v-for="item in relatedProducts" :key="item.id">
              <router-link :to="`/product/${ item.id }`" class="card h-100 justify-content-between">
                <!-- <div class="slide-img mt-auto" :style="{ backgroundImage: `url(${item.imageUrl})` }"></div> -->
                <div class="slide-hover">
                  <img :src="item.imageUrl" alt="商品" class="slide-img">
                </div>
                <div class="card-body h-100 d-flex flex-column justify-content-between">
                  <div>
                    <h4 class="card-title text-dark">{{ item.title }}</h4>
                    <p class="card-text text-dark swiper-overflow">{{ item.content }}</p>
                  </div>
                  <div>
                    <p class="card-text text-dark">NT${{ item.price }}</p>
                    <a href="#" class="btn btn-info text-light" @click.prevent="addToCart(item.id,qty)">加入購物車</a>
                  </div>
                </div>
              </router-link>
            </swiper-slide>
          </swiper>
          <div class="swiper-button-prev" ref="prevButton"  @click="onPrev">
            <!-- <img src="@/assets/images/images_swiper/arrow-left.png" /> -->
            <div class="border border-1 bg-info px-3 py-2" style="">
              <i class="d-block fa-solid fa-chevron-left text-light fs-5"></i>
            </div>
          </div>
          <div class="swiper-button-next"  ref="nextButton" @click="onNext">
            <!-- <img src="@/assets/images/images_swiper/arrow-right.png" /> -->
            <div class="border border-1 bg-info px-3 py-2" style="">
              <i class="fa-solid fa-chevron-right text-light fs-5"></i>
            </div>
          </div>
        </div>
      </section>
    </div>
  </div>
  </div>
</template>

<script>
import { Swiper, SwiperSlide } from 'swiper/vue'
import { Navigation, Autoplay } from 'swiper'
// loading
import Loading from 'vue-loading-overlay'
import 'vue-loading-overlay/dist/css/index.css'
// Import Swiper styles
import 'swiper/css'
import 'swiper/css/navigation'
import 'swiper/css/pagination'
import 'swiper/css/autoplay'
// pinia
import { mapActions } from 'pinia'
import cartStore from '../../store/cartStore.js'

const { VITE_APP_URL, VITE_APP_PATH } = import.meta.env
export default {
  data () {
    return {
      modules: [Navigation, Autoplay],
      products: [],
      relatedProducts: [],
      product: {
        qty: 1
      },
      // loding css
      color: '#9cd020',
      isLoading: false,
      fullPage: true,
      loader: 'bars'
    }
  },
  components: {
    Swiper, SwiperSlide, Loading
  },
  methods: {
    getProduct () {
      const { id } = this.$route.params
      this.$http.get(`${VITE_APP_URL}/v2/api/${VITE_APP_PATH}/product/${id}`)
        .then(res => {
          this.product = res.data.product
          this.getRelatedProducts()
          this.isLoading = false
        })
    },
    getProducts () {
      this.$http.get(`${VITE_APP_URL}/v2/api/${VITE_APP_PATH}/products/all`)
        .then((res) => {
          this.products = res.data.products
          this.isLoading = false
        })
    },
    getRelatedProducts () {
      this.products.forEach((item) => {
        if (item.category === this.product.category && item.id !== this.product.id) {
          this.relatedProducts.push(item)
        }
      })

      this.products.forEach((item) => {
        if (item.id !== this.product.id && this.relatedProducts.length < 3) {
          for (let i = this.relatedProducts.length; i < 3; i++) {
            let randomProduct
            do {
              randomProduct = this.products[Math.floor(Math.random() * this.products.length)]
            } while (this.relatedProducts.includes(randomProduct) || randomProduct.id === this.product.id)
            this.relatedProducts.push(randomProduct)
          }
        }
      })
    },
    doAjax () {
      this.isLoading = true
    },
    reproductQty (number) {
      this.product.qty += (number)
    },
    ...mapActions(cartStore, ['addToCart'], ['getCart'])
  },
  watch: {
    product: {
      handler () {
        // 使用 $nextTick 方法確保下拉選單已經渲染完畢再設定值
        this.$nextTick(() => {
          this.product.qty = this.product.qty || 1
        })
      },
      deep: true
    },
    $route () {
      window.location.reload()
    }
  },
  mounted () {
    this.doAjax()
    this.getProduct()
    this.getProducts()
  }
}
</script>

<style scoped>
.swiper {
  /* width: 600px; */
  width: 600px;
  height: 500px;
}
.swiper-slide img {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: cover;
}
</style>
