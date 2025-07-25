<template>
  <nav class="navbar" :class="{ 'is-scrolled': isScrolled }">
    <div class="navbar__container">
      <div class="navbar__logo">
        网站名
      </div>
    
      <button class="navbar__toggle" @click="open = !open">
        <span class="navbar__toggle-line"></span>
        <span class="navbar__toggle-line"></span>
        <span class="navbar__toggle-line"></span>
      </button>
      <ul class="navbar__links" :class="{ 'is-open': open }">
        <li v-for="item in links" :key="item.name" @click="open = !open">
          <router-link :to="item.path" class="link">{{ item.name }}</router-link>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script lang="ts" setup>
import { ref, onMounted, onUnmounted, computed } from 'vue';

const open = ref(false);
const isScrolled = ref(false);

const links = ref([
  { name: '页面1', path: '/' },
  { name: '页面2', path: '/page2' }, 
  { name: '页面3', path: '/page3' },
])
const handleScroll = () => {
  isScrolled.value = window.scrollY > 20;
};

onMounted(() => {
  window.addEventListener('scroll', handleScroll);
});
onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
});

</script>

<style lang="scss" scoped>
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  background: #000;
  backdrop-filter: blur(6px);
  transition: background 0.3s ease, box-shadow 0.3s ease;
  box-shadow: none;

  &.is-scrolled {
    background: #000;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
  }

  &__container {
    max-width: 1200px;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0.75rem 1rem;
  }

  &__logo {
    font-size: 1.5rem;
    font-weight: bold;
    color: #FFFFFF;
    text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.4);
  }



  &__toggle {
    display: none;
    flex-direction: column;
    justify-content: space-between;
    width: 24px;
    height: 18px;
    background: none;
    border: none;
    cursor: pointer;
    padding: 0;

    &-line {
      width: 100%;
      height: 2px;
      background: #FFFFFF;
      transition: transform 0.3s;
    }
  }

  &__links {
    display: flex;
    list-style: none;
    margin: 0;
    padding: 0;

    li {
      margin-left: 2rem;

      a {
        position: relative;
        color: rgba(255, 255, 255, 0.9);
        font-size: 1rem;
        font-weight: 500;
        text-decoration: none;
        padding: 0.25rem 0;
        transition: color 0.3s;

        &::after {
          content: '';
          position: absolute;
          bottom: -4px;
          left: 0;
          width: 0;
          height: 2px;
          background: #FFFFFF;
          transition: width 0.3s;
        }

        &:hover {
          color: #FFFFFF;

          &::after {
            width: 100%;
          }
        }

        &.router-link-active {
          color: #FFFFFF;

          &::after {
            width: 100%;
          }
        }
      }
    }

    /* 小屏幕折叠菜单 */
    @media (max-width: 768px) {
      position: absolute;
      top: 100%;
      left: 0;
      right: 0;
      background: #000;;
      flex-direction: column;
      overflow: hidden;
      max-height: 0;
      transition: max-height 0.3s ease;

      &.is-open {
        max-height: 300px;

        li {
          margin: 1rem 0;
          text-align: center;
        }
      }

      li {
        margin: 1rem 0;
        text-align: center;
      }
    }
  }

  /* 小屏幕展示汉堡 */
  @media (max-width: 768px) {
    &__toggle {
      display: flex;
    }

  }
}
</style>
