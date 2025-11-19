```c
/*给你一个整数数组 `citations` ，其中 `citations[i]` 表示研究者的第 `i` 篇论文被引用的次数。计算并返回该研究者的 **`h` 指数**。

根据维基百科上 [h 指数的定义](https://baike.baidu.com/item/h-index/3991452?fr=aladdin)：`h` 代表“高引用次数” ，一名科研人员的 `h` **指数** 是指他（她）至少发表了 `h` 篇论文，并且 **至少** 有 `h` 篇论文被引用次数大于等于 `h` 。如果 `h` 有多种可能的值，**`h` 指数** 是其中最大的那个。*/
//我的想法，很差的算法，时间复杂度n方 空间复杂度1
int hIndex(int* citations, int citationsSize) {
	int maxh = 0;
	for(int i = 0; i<citationsSize; i++){
		int count = 0;
		for(int j =0; j<citationsSize; j++){
			if(citations[j]>=i+1){
				count++;
			}
		}
		if(count>=i+1)maxh = i+1;
	}
	return maxh;
}
//排序，时间复杂度nlogn 空间复杂度 1
int cmp(const void* a, const void* b){
	int num1 = *(int*)a;
	int num2 = *(int*)b;
	return num1-num2;
	
}

int hIndex(int* citations, int citationsSize) {
	qsort(citations,citationsSize,sizeof(int),cmp);
	int h = 0, i = citationsSize-1;
	while(i>=0&&citations[i]>=h){
		h++;
		i--;
	}
	return h;
}

//新建数组，由于h不可能超过H，所以新建一个数组记录被引用i次的文章的篇数，大于H的按被引用H次处理
//时间复杂度n ，空间复杂度n，空间换时间
#include<stdlib.h>
int hIndex(int* citations, int citationsSize) {
	int* save = (int*)malloc(sizeof(int)*(citationsSize+1));
	for(int i = 0; i<=citationsSize; i++){
		save[i]=0;
	}
	for(int i = 0; i< citationsSize; i++){
		if(citations[i]>=citationsSize){
			save[citationsSize]++;
		}
		else{
			save[citations[i]]++;
		}
	}
	int h = 0;
	int total = 0;
	for(int i = citationsSize; i>0; i--){
		total+=save[i];
		if(total>=i){
			h = i;
			break;
		}
	}
	free(save);
	save =NULL;
	return h;
}
```





