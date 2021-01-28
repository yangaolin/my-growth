```
//获取侧边栏
if (sessionStorage.getItem(`${env}${empId}leftMenu`)) {
    const leftMenu = JSON.parse(sessionStorage.getItem(`${env}${empId}leftMenu`))
    store.dispatch('setLeftMenu', leftMenu)
} else {
    if (!store.state.leftMenu.length) {
      store.dispatch('setLeftLoading', true)
      axios(Api.leftMenu.getMenu(empId)).then(res => {
        if (res.data.code === 200) {
          store.dispatch('setLeftMenu', res.data.data)
          sessionStorage.setItem(`${env}${empId}leftMenu`, JSON.stringify(res.data.data))
          store.dispatch('setLeftLoading', false)
        }
      }).catch(e => {
        Common.handleError(e, store._vm)
      })
    }
}
```