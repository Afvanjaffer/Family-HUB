<style scoped>
  .photo {
    height: 200px;
  }
</style>

<template>
  <div>
    <p class="mb1 bb b--gray-test pa2">
      <span>
        <a v-if="gift.url" :href="gift.url" rel="noopener noreferrer" target="_blank">
          <strong>{{ gift.name }}</strong>
        </a>
        <strong v-else>{{ gift.name }}</strong>
      </span>

      <span v-if="gift.recipient && gift.recipient.complete_name">
        <span class="black-50 mr1 ml1">
          •
        </span>
        {{ $t('people.gifts_for', { name: gift.recipient.complete_name }) }}
      </span>
    </p>

    <!-- LIST OF PHOTO -->
    <div v-if="gift.photos !== undefined && gift.photos.length > 0" class="bb b--gray-test pa1">
      <div class="flex flex-wrap">
        <div v-for="photo in gift.photos" :key="photo.id" class="w-third-ns w-100">
          <div class="pa2 mb1 br2 ba b--gray-test" :class="dirltr ? 'mr3' : 'ml3'">
            <div class="cover bg-center photo w-100 h-auto br2 bb b--gray-test pb2"
                 :style="'background-image: url(' + photo.link + ');'"
                 @click.prevent="modalPhoto(photo)"
            >
            </div>
          </div>
        </div>
      </div>
    </div>

    <div v-if="gift.amount || gift.comment" class="f6 pv1 mb1 ph2 pb2 bb b--gray-test">
      <span v-if="gift.amount">
        {{ gift.amount_with_currency }}
      </span>
      <span v-if="gift.amount && gift.comment" class="black-50 mr1 ml1">
        •
      </span>
      <a v-if="gift.comment" class="pointer" href="" @click.prevent="comment = !comment">
        {{ $t('people.gifts_view_comment') }}
      </a>
      <div v-if="comment" class="mb1 mt1">
        {{ gift.comment }}
      </div>
    </div>

    <div class="ph2 pb2 cf f7">
      <slot></slot>
    </div>

    <!-- MODAL ZOOM PHOTO -->
    <sweet-modal ref="modalPhoto" tabindex="-1" role="dialog" :enable-mobile-fullscreen="true" width="33%">
      <img :src="url" :alt="$t('people.photo_title')" class="mw-90 h-auto mb3" />
    </sweet-modal>
  </div>
</template>

<script>

import { SweetModal } from 'sweet-modal-vue';

export default {

  components: {
    SweetModal,
  },

  props: {
    gift: {
      type: Object,
      default: null,
    },
  },

  data() {
    return {
      comment: false,
      showModal: false,
      url: '',
    };
  },

  computed: {
    dirltr() {
      return this.$root.htmldir == 'ltr';
    },
  },

  methods: {

    modalPhoto(photo) {
      this.url = photo.link;
      this.$refs.modalPhoto.open();
    },

  }
};
</script>
