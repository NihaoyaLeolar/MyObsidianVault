---
tags:
  - vue
---
当你使用了Vue.js的Vuex插件并创建了一个Vuex store，你可以在Vue组件中通过`this.$store`来访问和操作这个store中的状态（state）、触发mutations、以及执行actions。以下是一些基本的用法：

1. **读取状态（state）**：

   你可以通过`this.$store.state`来访问store中的状态。例如，假设你的Vuex store中有一个状态`currentStep`，你可以在组件中通过`this.$store.state.currentStep`来读取它的值。

   ```javascript
   // 读取当前步骤
   const currentStep = this.$store.state.currentStep;
   ```

2. **触发mutations**：

   你可以通过`this.$store.commit`来触发mutations。mutations是用来修改状态的函数。假设你有一个mutation函数叫`updateCurrentStep`，你可以在组件中通过`this.$store.commit('updateCurrentStep', newValue)`来触发它。

   ```javascript
   // 触发名为'updateCurrentStep'的mutation
   this.$store.commit('updateCurrentStep', newValue);
   ```

   在mutation函数中，你可以修改状态，然后所有引用这个状态的组件都会响应式地更新。

3. **执行actions**：

   你也可以通过`this.$store.dispatch`来执行actions。actions通常用于异步操作，然后再去触发mutations。假设你有一个action函数叫`fetchData`，你可以在组件中通过`this.$store.dispatch('fetchData', payload)`来执行它。

   ```javascript
   // 执行名为'fetchData'的action，payload是传递给action的参数
   this.$store.dispatch('fetchData', payload);
   ```

总之，通过`this.$store`，你可以访问和修改Vuex store中的状态，并且确保了全局状态的同步。这对于在多个组件之间共享数据和状态非常有用，特别是在大型Vue.js应用中。