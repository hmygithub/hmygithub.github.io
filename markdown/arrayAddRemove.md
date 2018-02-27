### I.新增：影响原数组

    let mutatingAdd=['a','b','c','d','e']
    mutatingAdd.push('f')
    console.log(mutatingAdd)
    mutatingAdd.unshift('z')//将新项添加到数组起始位置:
    console.log(mutatingAdd)

### II.新增：不影响原数组 

    *第一种是 array.concat()*
    const arr1 = ['a','b','c','d','e']
    const arr2 = arr1.concat('f')
    console.log(arr1)
    **第二种方法是使用 JavaScript 的展开（spread）操作符**
    const arr1 = ['a','b','c','d','e']
    const arr2 = [...arr1,'f']
    const arr3 = ['z',...arr1]
    console.log(arr2,arr3)

### III.移除：影响原数组

    *使用array.pop()和array.shift()移除数组元素时，会影响原来的数组*
    let mutatingRemove=['a','b','c','d','e']
    mutatingRemove.pop()
    console.log(mutatingRemove)
    mutatingRemove.shift()
    console.log(mutatingRemove)
    array.pop() 和 array.shift() 返回被移除的元素，你可以通过一个变量获取被移除的元素。

    let mutatingRemove = ['a', 'b', 'c', 'd', 'e'];
    const returnedValue1 = mutatingRemove.pop();  
    console.log(mutatingRemove); // ['a', 'b', 'c', 'd']  
    console.log(returnedValue1); // 'e'
    const returnedValue2 = mutatingRemove.shift();  
    console.log(mutatingRemove); // ['b', 'c', 'd']  
    console.log(returnedValue2); // 'a'
    array.splice()也可以删除数组的元素

    let mutatingRemove2=['a','b','c','d','e']
    // 像 array.pop() 和 array.shift() 一样，array.splice() 同样返回移除的元素。
    let returnedItems=mutatingRemove2.splice(0,2)
    console.log(mutatingRemove2)
    console.log(returnedItems)

### IV.移除：不影响原数组

    Javascript的Array.filter()方法基于原数组创建一个新数组，
    新数组仅包含匹配特定条件的元素
    const arr3 = ['a','b','c','d','e']
    // const arr4 = arr3.filter(a=> a!=='e')
    // console.log(arr4)
    // 或者
    const arr4=arr3.filter(a=>{
        return a!=='e'
    })
    console.log(arr4)

    另一种不影响原数组的方式是array.slice()
    const arr1 = ['a','b','c','d','e']
    const arr2 = arr1.slice(1,5)
    const arr3 = arr1.slice(2)
    console.log(arr2,arr3)

### V.替换：影响原数组

    如果知道替换哪一个元素，可以使用array.splice()
    let mutatingReplace=['a','b','c','d','e']
    // mutatingReplace.splice(2,1,30)
    // 或者
    mutatingReplace.splice(2,1,30,31)
    console.log(mutatingReplace)
    
    
### VI 替换：不影响原数组
    可以使用array.map()创建一个新数组，并且可以检查每一个元素，根据特定的条件替换它们
    const arr1 = ['a','b','c','d','e']
    const arr2 = arr1.map(item=>{
        if(item==='c'){
            item='CAT'
        }
        return item
    })
    console.log(arr2)
    使用array.map()转换数据
    array.map()是个强力方法，可以用于转换数据，而不污染原先的数据源
    const origArr = ['a','b','c','d','e']
    const transformedArr = origArr.map(n=>n+'Hi!')
    console.log(origArr)//原数组毫发无损
    
    var arr1 = "john".split('');
    // var arr2 = arr1.reverse();
    // var arr3 = "jones".split('');
    // arr2.push(arr3);
    // console.log("array 1: length=" + arr1.length + " last=" + arr1.slice(-1));
    // console.log("array 2: length=" + arr2.length + " last=" + arr2.slice(-1));