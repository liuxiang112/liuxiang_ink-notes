**对象数组排序：通过其的key值为数字的排序**

// 对包含对象的数组排序：按照keyName的大小排
````
  objectArraySort(keyName) {
    return function (objectN, objectM) {
      var valueN = objectN[keyName]
      var valueM = objectM[keyName]
      if (valueN < valueM) return -1
      else if (valueN > valueM) return 1
      else return 0
    }
  }
````
应用
````
arr.sort(objectArraySort(keyName))
````

