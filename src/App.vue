<template>
  <div id="app">
    <nav class="navbar">
      <div class="nav-container">
        <h1 class="nav-title">가계부 앱</h1>
        <div class="nav-links">
          <template v-if="userStore.accessToken">
            <router-link to="/" class="nav-link">홈</router-link>
            <router-link to="/transactions" class="nav-link">거래내역</router-link>
            <router-link to="/statistics" class="nav-link">통계</router-link>
            <router-link to="/budget" class="nav-link">예산</router-link>
            <router-link to="/groups" class="nav-link">그룹</router-link>
            <router-link to="/profile" class="nav-link">프로필</router-link>
            <button @click="logout" class="nav-link logout-button">로그아웃</button>
          </template>
          <template v-else>
            <router-link to="/login" class="nav-link">로그인</router-link>
            <router-link to="/register" class="nav-link">회원가입</router-link>
          </template>
        </div>
      </div>
    </nav>
    
    <main class="main-content">
      <router-view />
    </main>
    <NotificationModal v-if="userStore.accessToken" />
  </div>
</template>

<script setup>
import { onMounted } from 'vue'
import { useRouter } from 'vue-router' 
import { useUserStore } from '@/stores/user'
import axiosInstance from "@/service/axiosInstance"
import NotificationModal from '@/components/NotificationModal.vue'

const userStore = useUserStore()
const router = useRouter()

const logout = () => {
  userStore.logout()
  axiosInstance.setToken(null)
  router.push('/login')
}

onMounted(() => {
  axiosInstance.setToken(userStore.accessToken)
})
</script>
<!-- ...style은 기존과 동일 -->


<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f5f5f5;
  line-height: 1.6;
}

#app {
  min-height: 100vh;
}

.navbar {
  background-color: #2563eb;
  color: white;
  padding: 1rem;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.nav-container {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.nav-title {
  font-size: 1.5rem;
  font-weight: bold;
}

.nav-links {
  display: flex;
  gap: 2rem;
}

.nav-link {
  color: white;
  text-decoration: none;
  padding: 0.5rem 1rem;
  border-radius: 0.5rem;
  transition: background-color 0.2s;
}

.nav-link:hover {
  background-color: rgba(255,255,255,0.1);
}

.nav-link.router-link-active {
  background-color: rgba(255,255,255,0.2);
  font-weight: 600;
}

.main-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

@media (max-width: 768px) {
  .nav-container {
    flex-direction: column;
    gap: 1rem;
  }
  
  .nav-links {
    gap: 1rem;
    flex-wrap: wrap;
    justify-content: center;
  }
  
  .main-content {
    padding: 1rem;
  }
}

.logout-button {
  background: none;
  border: none;
  font-family: inherit;
  font-size: inherit;
  cursor: pointer;
}

.logout-button:hover {
  background-color: rgba(255,255,255,0.1);
}
</style>
