
### 1 冒泡排序

```
let arr = [5, 4, 3, 2, 1]
function sort(arr) {
	let temp;
	for (let i = 0; i < arr.length - 1; i++) {
		for (let j = 0; j < arr.length - 1 - i; j++){
			if(arr[j]>arr[j+1]){
				temp = arr[j]
				arr[j] = arr[j+1]
				arr[j+1] = temp
				<!-- [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]] -->
			}
		}
	}
	return arr
}
console.log(sort(arr)) //[1,2,3,4,5]
```

### 2 快速排序

```
let arr = [5, 4, 3, 2, 1]
function sort(arr) {
	if (arr.length <= 1) {
	  return arr;
	}
	var pivotIndex = Math.floor(arr.length / 2);
	var pivot = arr.splice(pivotIndex, 1)[0];
	var left = [];
	var right = [];
			
	for (var i = 0; i < arr.length; i++) {
	  if (arr[i] < pivot) {
	    left.push(arr[i]);
	  } else {
	    right.push(arr[i]);
	  }
	}
	return quickSort(left).concat([pivot], quickSort(right));
}
```