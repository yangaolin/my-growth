```
Vue.filter('money', (value, symbol = '', currency = '￥', decimals = 0) => {
  const digitsRE = /(\d{3})(?=\d)/g
  value = parseFloat(value)
  if (!isFinite(value) || (!value && value !== 0)) return ''
  currency = currency != null ? currency : '$'
  decimals = decimals != null ? decimals : 2
  let stringified = Math.abs(value).toFixed(decimals)
  let _int = decimals ?
    stringified.slice(0, -1 - decimals) :
    stringified
  let i = _int.length % 3
  let head = i > 0 ?
    (_int.slice(0, i) + (_int.length > 3 ? symbol : '')) :
    ''
  let _float = decimals ?
    stringified.slice(-1 - decimals) :
    ''
  let sign = value < 0 ? '-' : ''
  return sign + currency + head +
    _int.slice(i).replace(digitsRE, '$1,') +
    _float
})
Vue.filter('percent', value => { // 百分比
  return `${value}%`
})
Vue.filter('FormatMoney', (value, hasDecimal) => { // 千位数字以上*代替
  value = parseFloat(value).toFixed(hasDecimal)
  let result = '';
  let b = value.split(".")
  for (let i = 0; b[0].length > 3; i++) {
    if (i == 0) {
      result = ',' + b[0].slice(-3) + result
    } else {
      result = ',***' + result
    }
    b[0] = b[0].slice(0, b[0].length - 3)
  }
  if (b[0]) {
    if (result.length > 0) {
      for (var y = 0; b[0].length > y; y++) {
        result = "*" + result
      }
    } else {
      result = b[0] + result
    }
  }
  if (b[1]) {
    return result + '.' + b[1]
  }
  return result
})
Vue.filter('currency', (value, hasDecimal) => { // 数量格式化
  if (hasDecimal) {
    value = parseFloat(value).toFixed(hasDecimal)
  }
  return value && value.toString().replace(/(\d)(?=(\d\d\d)+(?!\d))/g, '$1,') || 0
})
Vue.filter('time', (value, mark = '-') => {
  if (!value)
    return
  let time = new Date(value)
  return moment(time).format(`YYYY${mark}MM${mark}DD`)
})
```