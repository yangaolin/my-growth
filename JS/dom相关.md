#### 一个dom被删除 再被添上时 用this.$refs.stripOuter的方式 取高度 不能正常的取值 用this.$nextTick 也不行 这时 要用document.getElementsByClassName('checkHeader')[0]的方式
