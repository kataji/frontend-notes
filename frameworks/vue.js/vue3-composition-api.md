# Vue3 Composition API

[https://v3.vuejs.org/guide/composition-api-introduction.html](https://v3.vuejs.org/guide/composition-api-introduction.html)

看了一些消息，似乎是想代替原来的options API的。不过囿于现状，还得和原来的兼容共存。

看样子是以一个新的方式定义data，computed，watch，methods，lifecycle hooks之类的东西，例如：

```javascript
// MyBook.vue
import { ref } from 'vue'


export default {
  // setup的调用时机类似beforeCreate(?)，此时没有vm的“this”
  setup(props) {
    const repositories = ref([])
    const getUserRepositories = async () => {
      repositories.value = await fetchUserRepositories(props.user)
    }
  
    return {
      repositories, // 一个state
      getUserRepositories // 一个method
      // 是不是很像react的hook
    }
  }
}
```

这个做法的优点如其名，可以做sub-component级别的拆分和组合，便于组织逻辑。例如：

把一个功能相关的逻辑（数据、方法等）放在一个文件里，写成一个composition function：

```javascript
// src/composables/useUserRepositories.js

import { ref, onMounted, watch } from 'vue'

export default function useUserRepositories(user) {
  const repositories = ref([])
  const getUserRepositories = async () => {
    repositories.value = await myFetchUserRepositories(user.value)
  }

  onMounted(getUserRepositories)
  watch(user, getUserRepositories)

  return {
    repositories,
    getUserRepositories
  }
}
```

在component里引入并组织这些composition functions：

```javascript
// src/components/UserRepositories.vue
import { toRefs } from 'vue'
import useUserRepositories from '@/composables/useUserRepositories'
import useRepositoryNameSearch from '@/composables/useRepositoryNameSearch'
import useRepositoryFilters from '@/composables/useRepositoryFilters'

export default {
  components: { RepositoriesFilters, RepositoriesSortBy, RepositoriesList },
  props: {
    user: {
      type: String,
      required: true
    }
  },
  setup(props) {
    const { user } = toRefs(props)

    const { repositories, getUserRepositories } = useUserRepositories(user)

    const {
      searchQuery,
      repositoriesMatchingSearchQuery
    } = useRepositoryNameSearch(repositories)

    const {
      filters,
      updateFilters,
      filteredRepositories
    } = useRepositoryFilters(repositoriesMatchingSearchQuery)

    return {
      // Since we don’t really care about the unfiltered repositories
      // we can expose the end results under the `repositories` name
      repositories: filteredRepositories,
      getUserRepositories,
      searchQuery,
      filters,
      updateFilters
    }
  }
}
```

setup函数返回的属性可以作为属性和方法在template里使用。

在setup函数之外，仍然可以像以往那样定义options，在option里也可以使用setup返回的属性们。

