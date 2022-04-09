<template>
  <div class="home">
  </div>
</template>

<script>
// src/components/UserRepositories.vue `setup` function
import { fetchUserRepositories } from '@/api/repositories'
import { ref, onMounted, watch, toRefs, computed } from 'vue'
export default {
  name: 'HomeView',
  setup(props) {
  // 使用 `toRefs` 创建对 `props` 中的 `user` property 的响应式引用
    const { user } = toRefs(props)
    
    const repositories = ref([])
    const getUserRepositories = async () => {
      // 更新 `props.user ` 到 `user.value` 访问引用值
      //repositories.value = await fetchUserRepositories(props.user)
      repositories.value = await fetchUserRepositories(user.value)
    }
    onMounted(getUserRepositories)
    // 监听user的变化，去获取用户仓库信息
    watch(user, getUserRepositories)
    
    const searchQuery = ref('')
    //根据查询的关键字筛选出对应的结果
    const repositoriesMatchingSearchQuery = computed(() => {
      return repositories.value.filter(
        repository => repository.name.includes(searchQuery.value)
      )
    })
    
    
    return {
      repositories,
      getUserRepositories,
      searchQuery,
      repositoriesMatchingSearchQuery
    }
  }
}
</script>
