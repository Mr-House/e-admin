<template>
  <ea-admin @resize="resize">
    <template v-slot:header>
      <ea-header
        :collapse.sync="headerCollapse"
        :notice="notice"
        :fullscreen="fullscreen"
        :nav-data="headerNavData"
        :user="user"
        @command="handleCommand"
      >
        <div style="flex:1"/>
        <div class="ea-hover">
          <a class="iconfont icon-github" target="_blank" href="https://github.com/akebe/e-admin"/>
        </div>
      </ea-header>
    </template>
    <template v-slot:tabs>
      <ea-tabs
        :collapse.sync="tabsCollapse"
        :nav-data="tabsNavData"
        storage
      />
    </template>
    <template v-slot:side>
      <ea-logo
        v-if="logoVisible"
        :name="name"
        :logo="logo"
        logo-type="image"
      />
      <ea-nav-menu
        v-model="active"
        router
        :data="navData"
      />
    </template>
  </ea-admin>
</template>
<script>
  import navData from './nav-data';
  import logo from '@/assets/logo.svg';
  import store from '@/store';
  import {version} from '../../package.json';

  export default {
    name: 'Index',
    components: {},
    props: {},
    watch: {},
    data() {
      return {
        user: store.user,
        unread: 10,
        collapse: false,
        active: '',
        name: `e-admin`,
        version,
        logo,
        navData,
        tabsCollapseActive: true,
        tabsNavDataActive: true,
        notice: '',
        headerCollapseActive: false,
        fullscreen: true,
        headerNavDataActive: true,
        logoVisible: true,
      };
    },
    computed: {
      tabsCollapse: {
        get() {
          return this.tabsCollapseActive ? this.$ea.config.collapse : 'false';
        },
        set(v) {
          this.$ea.config.collapse = v;
        },
      },
      tabsNavData() {
        return this.tabsNavDataActive ? navData : [];
      },

      headerCollapse: {
        get() {
          return this.headerCollapseActive ? this.$ea.config.collapse : 'false';
        },
        set(v) {
          this.$ea.config.collapse = v;
        },
      },
      headerNavData() {
        return this.headerNavDataActive ? navData : [];
      },
    },
    methods: {
      handleCommand(command) {
        switch (command) {
          case 'me':
            this.$message.warning('未实现');
            break;
          case 'setting':
            this.$message.warning('未实现');
            break;
          case 'logout':
            store.user = {
              name: '',
              id: '',
            };
            this.$message.success('已退出');
            localStorage.setItem('document_user', JSON.stringify(store.user));
            this.$router.push('/login');
            break;
        }
      },
      resize() {
        // eslint-disable-next-line no-console
        console.log('ea-resize');
      },
    },
    created() {
    },
    mounted() {
    },
  };
</script>
<style scoped>
  .icon-github {
    font-size: 22px
  }
</style>