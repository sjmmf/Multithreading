```c

#include <pthread.h>
#include <stdio.h>
#include <unistd.h>
#include<stdbool.h>
#include<sys/types.h>

#define MAX  100       //数字上限
#define OVER '0'       //判断过的标志
#define NEVER '1'      //还未判断过的标志
char buf[MAX];         //用于存放数字的判断结果
int j = 2;             //外部的计数器

/*此函数用于对buf数组初始化*/
void init_buf(char* str) {
	int i;
	for (i = 0; i < MAX; i++) {
		str[i] = NEVER;
	}
	return;
}

/*此函数用于判断是否所有数字都进行过素数判断*/
bool judge_all_over(char* str) {
	int i;
	for (i = 0; i < MAX; i++) {
		if (str[i] == NEVER) {
			return false;
		}
	}
	return true;
}

/*此函数用于表示判断素数的进程*/
void* prime(void* arg) {
	while (judge_all_over(buf)) {
		while (buf[j] == OVER) {
			j++;
		}
		printf("Prime:%d\n", j);
		buf[j] = OVER;
		int m;
		for (m = j + 1; m < MAX; m++) {
			if (m % j == 0) {
				buf[m] = OVER;
			}
		}
	}
	
}

int main() {
	init_buf(buf);
	pthread_t thread;
	pthread_create(&thread, NULL, prime, NULL);
	pthread_join(thread, NULL);

	return 0;
}
```

