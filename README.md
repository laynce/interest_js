## 结合应用场景加入应用实例
### 收集有趣的题型，感兴趣的快来围观

### Proxy的使用

- 例题1,实现一个pipe函数，满足以下输出  


  ```
    var add = (n) => n + 2
    var sub = (n) => n - 2
    var double = (n) => n * 2
    var pow = (n) => Math.pow(n, n)
    
      
    const res1 = pipe(2).double.pow.do      // 256
    const res2 = pipe(4).add.double.do      // 12
    const res3 = pipe(6).sub.add.double.do  // 12
    console.log(res1, res2, res3)

    var pipe = (num) => {
        const fnArr = []
        return new Proxy({}, {
          get(target, name, proxyObj){
          
            if (name === 'do') {
              return fnArr.reduce((total, fn)=> {
                return window[fn](total)
              }, num)
            }else {
              fnArr.push(name)
              return proxyObj
            }
          }
        })
      }
  ```

- 例题2, 实现add, 满足以下求和场景, ([Symbol.toPrimitive](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/toPrimitive)点击详解)


  ```
    const add = new Proxy({sum: 0}, {
      get(target, property, receiver) {

        if(property === Symbol.toPrimitive) {
          const tmp = target.sum
          target.sum = 0
          return () => tmp
        }
          
        target.sum += Number(property)
      
        return receiver
      }
    })

    const r1 = add[1][2] + 9
    const r2 = add[10][20] + 9
    const r3 = add[100][200][3000] + 9
    console.log(r1)
  ```
  
