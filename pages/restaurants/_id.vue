// pages/restaurants/_id.vue
    <template>
      <div class="uk-container uk-container-xsmall">
        <span class="uk-heading-small">
          // Link to go back to the previous page
          <NuxtLink class="uk-button uk-button-text" to="/">
            <span uk-icon="arrow-left"></span> go back
          </NuxtLink>
        </span>
    
        // Displaying dishes
        <div v-for="dish in dishes" :key="dish.id">
          <div class="uk-card uk-card-default" style="border-top: 3px dotted #000; padding-bottom: 30px;" uk-grid>
            <figure class="uk-card-media-top uk-cover-container" >
              <!-- <div v-for="image in dishes" :key="image.id"> -->
              
                <!-- <div v-if="image == image.imageIndex"> -->
                  <VueSlickCarousel v-bind="settings">
                  <!-- <vue-picture-swipe: items="items"> -->
                    
                  <template v-if="dish.attributes.image">
                      <div v-for="(imageObj, imageIndex) in dish.attributes.image.data" :key="imageIndex">
                        <img  class="group list-group-image"
                        :src="getStrapiMedia(imageObj.attributes.url)"  
                        :alt="imageObj.attributes.alternativeText" /> 
              
                      </div>
                  </template>
                  <!-- </vue-picture-swipe> -->
                   </VueSlickCarousel> 
            </figure>
            <div class="uk-card-body uk-card-small" style="padding-left: 50px; padding-right: 30px;">
              <h2 class="uk-card-title">{{ dish.attributes.name }}</h2>
              <!-- <p>{{ restaurant.data.attributes.name }}</p> -->
              <p>{{ dish.attributes.description }} </p>
              <p>{{ dish.attributes.price }} $</p>
              <button class="uk-button uk-button-primary uk-margin-xlarge-top">
                Add to cart
              </button>
            </div>
            

          </div>
        </div>
      </div>
    </template>
    
    <script>
    // import PhotoSwipeLightbox from 'photoswipe/lightbox';
    // import 'photoswipe/style.css';
     import VueSlickCarousel from 'vue-slick-carousel' 
     import 'vue-slick-carousel/dist/vue-slick-carousel.css'
    // optional style for arrows & dots
     import 'vue-slick-carousel/dist/vue-slick-carousel-theme.css'
    import { getStrapiMedia } from '@/utils/media'
    import restaurantQuery from '@/apollo/queries/restaurant'
    

      /* const lightbox = new PhotoSwipeLightbox({
        gallery: '#my-gallery',
        children: 'a',
        pswpModule: () => import('photoswipe')
      });
      lightbox.init(); */
    
    
    export default {

         /* name: 'SimpleGallery',
      props: {
        galleryID: String,
        images: Array,
      },
      setup(props) {
        return {
          imagesData: props.images,
        };
      },
      mounted() {
        if (!this.lightbox) {
          this.lightbox = new PhotoSwipeLightbox({
            gallery: '#' + this.$props.galleryID,
            children: 'a',
            pswpModule: () => import('photoswipe'),
          });
          this.lightbox.init();
        }
      },
      unmounted() {
        if (this.lightbox) {
          this.lightbox.destroy();
          this.lightbox = null;
        }
      },  */

       components: {VueSlickCarousel},
      
      data() {
        return {
          restaurant: Object,
          settings: {
        "dots": true,
  "infinite": true,
  "initialSlide": 2,
  "speed": 500,
  "slidesToShow": 1,
  "slidesToScroll": 1,
  "swipeToSlide": true
}
        }
        
      },
      apollo: {
        restaurant: {
          prefetch: true,
          query: restaurantQuery,
          variables() {
            return { id: this.$route.params.id }
          },
        },
      },
      computed: {
        dishes() {
          if (!this.restaurant?.data) return [];

          /* <!-- this.restaurant?.data
          asd(this.restaurant ? this.restaurant.data : undefined)
          let asd = this.restaurant ? this.restaurant.data : undefined --> */
          return this.restaurant.data.attributes.dishes.data
        },
      },
      methods: {
        getStrapiMedia, 
      },
      
    }
    </script>

    

    <style>
    .slick-arrow {
      color:gray;
    }
    .slick-arrow::before {
      color:black;
    }
    .uk-card-media-top {
      padding-right: 40px;
    padding-left: 50px;
    }
   
    /* .uk-card {
      padding-right: 40px;
    padding-left: 40px;
    } */
    </style>

    
      