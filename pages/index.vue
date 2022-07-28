// pages/index.vue



    <template>
    
    <div>
      <div class="container" style="text-align: center; background: linear-gradient(0.25turn, #3f87a6, #ebf8e1, #42f2cded);">
      <header>
        <div class="container" >
            <div class="row">
                <div class="col-12 col-lg-5 col-xl-6 text-center text-lg-left">
                    <a href="/"><img id="logo" src="/media/Fisherbiltlogo.png" alt="Fisherbilt"></a>
                    <img src="https://raw.githubusercontent.com/SergeiIushkov/Fisherbilt/master/media/fisherbiltlogo1.png" alt="Fisherbilt" class="d-none d-md-inline">
                </div>
                <div class="col-md-6 col-lg-2 col-xl-2 align-self-center text-center text-md-right text-lg-left">

                    <a href="tel:+12549475868">
                      <!-- <span class="uk-margin-small-right" uk-icon="check" > Call us</span> -->
                        <a href="" uk-icon="receiver"></a>
                        <strong>(254) 947-5868</strong>
                    </a>
                </div>
                <div class="col-md-6 col-lg-5 col-xl-4 align-self-center  text-center text-md-left">
                    <a href="/contact-us">
                      <!-- <span class="uk-margin-small-right" uk-icon="check" ></span> -->
                        <a href="" uk-icon="location"></a>
                         <strong>7400 S IH 35 Service Rd, Belton, TX 76513</strong>
                    </a>
                </div>

            </div>
        </div>

    </header>
    

    <nav class="uk-navbar-container" uk-navbar style="background: linear-gradient(0.25turn, #3f87a6, #ebf8e1, #42f2cded)">
      <div class="uk-navbar-center">

        <ul class="uk-navbar-nav">
            <li class="uk-active"><a href="#">Active</a></li>
            <li>
                <a href="#">Parent</a>
                <div class="uk-navbar-dropdown">
                    <ul class="uk-nav uk-navbar-dropdown-nav">
                        <li class="uk-active"><a href="#">Active</a></li>
                        <li><a href="#">Item</a></li>
                        <li><a href="#">Item</a></li>
                    </ul>
                </div>
            </li>
            <li><a href="#">Item</a></li>
        </ul>

      </div>
     </nav>
     </div>
      <div class="uk-container uk-container-small">
        <h1 class="uk-heading-small">
          <span class="uk-invisible">Restaurants</span>
            <div style="border: 1px solid black;"><input
            v-model="query"
            class="uk-search-input"
            type="search"
            placeholder=" TYPE TO SEARCH"
          />
            </div>
        </h1>
    
        <div
          v-for="restaurant in filteredList"
          :key="restaurant.id"
          >
          <div class="uk-child-width-1-2@m" style="
    border-top: 3px dotted #000;
    padding-bottom: 20px;
" uk-grid>
          <!-- <div
            class="uk-card uk-card-default" uk-grid> -->
            <!-- <div class="uk-card-body uk-card-small"> -->
              
            <div class="uk-body">
                <NuxtLink
                  h2 class="uk-button uk-button-text"
                  :to="{ name: 'restaurants-id', params: { id: restaurant.id } }"
                >
                  <h2 class="uk-card-title">{{ restaurant.attributes.name }}</h2>
                </NuxtLink>
                <NuxtLink
                  class="uk-button uk-button-text"
                  :to="{ name: 'restaurants-id', params: { id: restaurant.id } }"
                >
                  (Detailed description)
                </NuxtLink>
            </div>
              <div class="uk-card-media-bottom uk-cover-container" style="
    margin-bottom: 40px;
">
                <VueSlickCarousel v-bind="settings">
            <img
              :src="getStrapiMedia(restaurant.attributes.image.data.attributes.url)"
              :alt="restaurant.attributes.image.data.attributes.alternativeText"
              uk-cover
            />
                </VueSlickCarousel>
              </div>
          
            <!-- </div> -->
            
            
          
          </div>
        </div>
        <div v-if="filteredList.length == 0" class="uk-heading-small">
          <p>No restaurants found</p>
        </div>
      </div>
      </div>
    </template>
    
    
    <script>
    import { getStrapiMedia } from '@/utils/media'
    import restaurantsQuery from '@/apollo/queries/restaurants'
    export default {
      data() {
        return {
          restaurants: [],
          query: '',
          settings: {
        "dots": true,
  "infinite": true,
  "initialSlide": 1,
  "speed": 500,
  "slidesToShow": 1,
  "slidesToScroll": 1,
  "swipeToSlide": true
}
        }
      },
      apollo: {
        restaurants: {
          prefetch: true,
          query: restaurantsQuery,
        },
      },
      computed: {
        filteredList() {
          if (!this.restaurants?.data) return []
          return this.restaurants?.data?.filter((restaurant) => {
            return restaurant.attributes.name
              .toLowerCase()
              .includes(this.query.toLowerCase())
          })
        },
      },
      methods: {
        getStrapiMedia,
      },
    }
    </script>

    <style>
    .uk-card-media-bottom{
      height:300px;
    }

    /* .uk-navbar-container:not(.uk-navbar-transparent) {
    background: #76e5fc;
} */

    a, .uk-link{
      color: #000000;
    }

    </style>