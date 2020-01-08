
#### JavaScript 

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    let index1 = 0;
    let index2 = 0;
	let hit = false;
    for (var i = 0; i < nums.length; i ++) {
    	let sum = 0;
    	index1 = i;
    	for (var j = 0; j < nums.length; j ++) {
    		index2 = j;
    		if (i != j && (nums[i] + nums[j]) == target) {
    			hit = true;
    			break;
    		} else {
    			continue;
    		}
    	}
        if (hit === true) {
            break;
        }
    }
     
	if (hit === true) 
    	return [index1, index2];
	else 
		return [];
};
```

**Test**
```
twoSum([3,2,4], 6);
twoSum([2, 7, 11, 15], 9);
```
