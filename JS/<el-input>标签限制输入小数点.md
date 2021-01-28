#### 使用进行金额输入，所以需要将type设置为number类型，但是会发现，字母e和小数点.还是可以输入，为了达到限制输入，需要做以下处理。

```
 <el-input size="medium"  type="number" placeholder="其他充值金额" v-model="inputMony" @focus="inputFocus" @keydown.native="channelInputLimit">

 // bug fix：指定输入类型为number时仍然可以输入字母'e'和小数点'.'（因为也属于数字类型的范围），这里做一下输入限制
    channelInputLimit (e) {
      let key = e.key
      // 不允许输入'e'和'.'
      if (key === 'e' || key === '.') {
        e.returnValue = false
        return false
      }
      return true
    }
```