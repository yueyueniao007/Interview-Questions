
### 1 冒泡排序

#### 1

```
let arr = [5, 4, 3, 2, 1]
function sort() {
	let temp;
	for (let i = 0; i < arr.length - 1; i++) {
		for (let j = 0; j < arr.length - 1 - i; j++){
			if(arr[j]>arr[j+1]){
				temp = arr[j]
				arr[j] = arr[j+1]
				arr[j+1] = temp
			}
		}
	}
	return arr
}
console.log(sort(arr)) //[1,2,3,4,5]
```