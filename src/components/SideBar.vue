<template>
  <div
    class="flex font-lato transform transition fixed w-60"
    style="min-width: 7rem;"
  >
    <section
      class="text-center bg-yellow-500 text-black p-4 mb-10 h-screen border-r-8 border-black flex flex-col"
      :class="{ 'hidden': !isMenuOpen}"
    >
<<<<<<< HEAD
      <div class="mb-8"><Select ref="languageSelect" @onChange="changeLang" :options="languageList" :defaultWord="'Please select a language'" /></div>
=======
         
      <div class="mb-8">
        <Select ref="languageSelect" @onChange="changeLang" :options="languageList" :defaultWord="'Please select a language'" />
        </div>
>>>>>>> 953684c... [feature]Add People page
      <div v-for="menu in menuContent" :key="menu" class="text-2xl mb-4" @click="closeMenu">
        <router-link v-if="menu.enabled" :to="`${menu.to}`">
          <h4 class="border-black" :class="[isCurrentPage(menu.to) ? 'border-b-4' : '']">{{ menu.name }}</h4>
        </router-link>
      </div>
      <div class="mt-auto mb-4" @click="closeMenu">
        <router-link to="/">
          <img class="w-full" src="../assets/images/logo.svg" alt="africa-africa-logo"/>
        </router-link>
      </div>
      <div class="">Copyright © Rachel &#129419</div>
    </section>
    <div class="transition absolute w-28" :class="[{'opacity-50': !isMenuOpen && !isLogoHover}, {'left-60': isMenuOpen}, {'left-0': isMenuOpen}  ]" 
    @mouseover="isLogoHover = true" @mouseleave="isLogoHover = false">
      <router-link @click="closeMenu" to="/" v-if="!isMobile" id="logoIcon">
          <img v-if="!isOutOfHeader" class="mb-4 mt-4 w-28" src='../assets/images/logo.svg' style="height: 60px" alt="africa-africa-logo"/>
          <img v-if="isOutOfHeader" class="mb-4 mt-4 w-28" src='../assets/images/logo-black.svg' style="height: 60px" alt="africa-africa-logo"/>
        
      </router-link>
      <img
        @click="toggleMenu"
        class="icon cursor-pointer transform transition"
        :class="{ 'rotate-180': !isMenuOpen }"
        src="../assets/images/arrow.svg"
        alt="africa-africa-logo"
      />
    </div>
  </div>
</template>
<script>
import Select from "@/components/Select";
export default {
  name: "SideBarr",
  components: {Select,},
  emits: ["ismenuopen"],
  data() {
    return {
      isOutOfHeader: false,
      isMenuOpen: true,
      isLogoHover: false,
      currentPath:'',
      isMobile: false,
      languageList: [{name:'English', value:'en'},{name:'繁體中文', value:'zh-TW' },{name:'简体中文', value:'zh-CN'}],
      menuContent: [
        { name: "Home", to: "/", enabled: true, },
        { name: "Learn from map", to: "/map", enabled: true, },
        { name: "Africa News", to: "/news", enabled: false, },
        { name: "lights to Africa", to: "/", enabled: false },
        { name: "Books about Africa", to: "/", enabled: false },
        { name: "Game time!", to: "/", enabled: false },
      ],
    };
  },
  methods: {
    toggleMenu() {
      this.isMenuOpen = !this.isMenuOpen;
      this.$store.commit('setMenuOpen', this.isMenuOpen);
    },
    isCurrentPage(to){
      return this.currentPath === to
    },
    closeMenu(){
      console.log('closeMenu');
      // if(!this.isMobile) return;
      if(!this.$_isMobile()) return;
      this.isMenuOpen = false;
      this.$store.commit('setMenuOpen', this.isMenuOpen);
    },
    changeLang(value){
      this.$i18n.locale = value;
    },
    addLogoStyle(){
      const header = document.querySelector('header')
      const logoIcon = document.querySelector('#logoIcon')
      const headerHeight = header.clientHeight
      const logoIconHeight = logoIcon.getBoundingClientRect().height
      const targetHeight = headerHeight - logoIconHeight
    
        const height = window.pageYOffset ||document.documentElement.scrollTop ||document.body.scrollTop    
        if(height > targetHeight){
          this.isOutOfHeader = true
        } 
        else{
          this.isOutOfHeader = false
        }
      }
  },
  watch: {
    routerName: {
      handler() {
        this.currentPath = this.$router.currentRoute._rawValue.path;
      },
      deep: true,
    },
    checkIsMobile:{
      handler() {
        this.isMobile = this.$_isMobile();
        if(this.isMobile){
          this.closeMenu();
        }

      },
      deep: true,
      immediate: true,
    }
  },
  computed: {
    routerName(){
      return this.$router;
    },
    checkIsMobile(){
      return this.$_isMobile();
    }
  },
  mounted(){
    window.addEventListener('resize', this.closeMenu);
    this.$refs.languageSelect.selected = 'en';

    this.$nextTick(()=>{
      if(this.$_isMobile()) return 

      document.addEventListener('scroll', this.addLogoStyle)

    })
  },
};
</script>
<style lang="css" scoped>
.icon {
  width: 30px;
}
</style>
