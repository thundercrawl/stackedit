<template>
  <modal-inner class="modal__inner-1--sponsor" aria-label="Sponsor">
    <div class="modal__content">
      <p>Please choose a <b>PayPal</b> option.</p>
      <a class="paypal-option button flex flex--row flex--center" v-for="button in buttons" :key="button.id" :href="button.link">
        <div class="flex flex--column">
          <div>{{button.price}}<div class="paypal-option__offer" v-if="button.offer">{{button.offer}}</div></div>
          <span>{{button.description}}</span>
        </div>
      </a>
    </div>
    <div class="modal__button-bar">
      <button class="button" @click="config.reject()">Cancel</button>
    </div>
  </modal-inner>
</template>

<script>
import { mapGetters } from 'vuex';
import ModalInner from './common/ModalInner';
import utils from '../../services/utils';

export default {
  components: {
    ModalInner,
  },
  data() {
    const sponsorToken = this.$store.getters['workspace/sponsorToken'];
    const makeButton = (id, price, description, offer) => {
      const params = {
        cmd: '_s-xclick',
        hosted_button_id: id,
        custom: sponsorToken.sub,
      };
      return {
        id,
        price,
        description,
        offer,
        link: utils.addQueryParams('https://www.paypal.com/cgi-bin/webscr', params),
      };
    };

    return {
      buttons: sponsorToken ? [
        makeButton('TDAPH47B3J2JW', '$5', '3 months sponsorship'),
        makeButton('6CTKPKF8868UA', '$15', '1 year sponsorship', '-25%'),
        makeButton('A5ZDYW6SYDLBE', '$25', '2 years sponsorship', '-37%'),
        makeButton('3DMD3TT2RDPQA', '$50', '5 years sponsorship', '-50%'),
      ] : [],
    };
  },
  computed: {
    ...mapGetters('modal', [
      'config',
    ]),
  },
  methods: {
    sponsor() {
    },
  },
};
</script>

<style lang="scss">
@import '../common/variables.scss';

.modal__inner-1--sponsor {
  max-width: 380px;
}

.paypal-option {
  text-align: center;
  padding: 10px;
  height: auto;
  font-size: 2.3em;
  margin: 0.75rem 0;
  line-height: 1.2;
  text-transform: none;

  span {
    display: inline-block;
    font-size: 0.75rem;
    opacity: 0.5;
    white-space: normal;
    line-height: 1.5;
  }

  .paypal-option__offer {
    float: right;
    font-size: 0.6rem;
    font-weight: 600;
    padding: 0.1em 0.2em;
    background-color: darken($error-color, 10);
    border-radius: 3px;
    color: #fff;
    margin-left: -0.5em;
  }
}
</style>
