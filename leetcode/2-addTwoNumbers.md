```
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function(l1, l2) {

    function genNumFromList(listHead) {
        return genValuesFromListHead(listHead).join('');
    }

    function genListFromNum(n) {
        let nodes = [];
        let node = null;
        let arr = new String(n).split('');//1,8
        let next = null;
        for (var i = 0; i < arr.length; i ++) {
            node = new ListNode(Number.parseInt(arr[i]));
            if (i > 0) {
                node.next = next;
            }
            nodes.push(node);
            next = node;
        }
        return nodes[nodes.length - 1];
    }

    function genValuesFromListHead(listHead) {
        let array = [];
        let values = [listHead.val];
        let next = listHead.next;
        while (next) {
            values.push(next.val);
            next = next.next;
        }
        return values.reverse();
    }

    function sumBigString(s1, s2) {
        const n1 = s1.split('');
        const n2 = s2.split('');
        let sumList = [];
        let increase = 0;
        let sum = 0;
        let a = 0;
        let b = 0;
        let k1 = n1.length - 1;
        let k2 = n2.length - 1;
        const length = n1.length > n2.length? n1.length : n2.length;
        for (var i = 0; i < length; i ++) {
            if (i >= n1.length) {
                a = 0;
            } else {
                a = Number.parseInt(n1[k1 - i]);
            }
            if (i >= n2.length) {
                b = 0;
            } else {
                b = Number.parseInt(n2[k2 - i]);
            }
            sum = a + b + increase;
            // console.log('--round add: ', a, b, increase);
            if (sum > 9) {
                increase = Number.parseInt(sum / 10);
                sum = sum % 10;
            } else 
                increase = 0;

            sumList.push(sum);
        }
        if (increase > 0) {
            sumList.push(1);
        }
        return sumList.reverse().join('');
    }

    // console.log(l1, l2);
	let n1 = genNumFromList(l1);
	let n2 = genNumFromList(l2);

	const sum = sumBigString(n1, n2);
	console.log(n1, n2, sum);

	const l3 = genListFromNum(sum);
    console.log(l3);
    return l3;
    
};
```



Test:

```
addTwoNumbers(genListHeadByArray([2,4,3]), genListHeadByArray([5,6,4])); // expected: [7,0,8]
addTwoNumbers(genListHeadByArray([1,8]), genListHeadByArray([0]));// expected: [1,8]
addTwoNumbers(genListHeadByArray([5]), genListHeadByArray([5]));// expected: [0,1]
addTwoNumbers(genListHeadByArray([1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1]), genListHeadByArray([5,6,4])); //Expected: [6,6,4,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1]
```

