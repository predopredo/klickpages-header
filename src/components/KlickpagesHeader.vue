<template>
  <div
    class="navbar"
    :class="{'hotmart-pro': isHotmartPro}"
    role="navigation"
    v-if="settingsLoaded">
    <hotmart-header
      v-if="isHotmartPro"
      :hotmartSrc="hotmartURL"
    />
    <menu-switch />
    <div class="container no-padding">
      <home-link />
      <nav-link-list />
      <notification-dropdown />
      <menu-user-dropdown />
    </div>
  </div>
</template>

<script>
import { mapActions, mapGetters } from 'vuex';
import HotmartHeader from '@/components/HotmartHeader.vue';
import MenuSwitch from './MenuSwitch.vue';
import HomeLink from './HomeLink.vue';
import NavLinkList from './NavLinkList.vue';
import MenuUserDropdown from './MenuUserDropdown.vue';
import NotificationDropdown from './notification/NotificationDropdown.vue';

import { setklickartURL } from '../config/klickart';
import { setJWTSecret } from '../config/jwt';

export default {
  name: 'KlickpagesHeader',
  props: {
    klickartURL: String,
    jwtSecret: String,
    hotmartURL: String,
  },
  data() {
    return {
      settingsLoaded: false,
      isHotmartPro: false,
    };
  },
  components: {
    HotmartHeader,
    MenuSwitch,
    NavLinkList,
    HomeLink,
    MenuUserDropdown,
    NotificationDropdown,
  },
  methods: mapActions({
    getTopBarConfig: 'topBar/getConfig',
  }),
  computed: mapGetters({
    topBarConfig: 'topBar/config',
  }),
  async mounted() {
    setklickartURL(this.klickartURL);
    setJWTSecret(this.jwtSecret);
    await this.getTopBarConfig()
      .catch((error) => {
        console.error(error);
      });

    if (this.topBarConfig.user.ucode) {
      this.isHotmartPro = true;
    }
    this.settingsLoaded = true;
  },
};
</script>
