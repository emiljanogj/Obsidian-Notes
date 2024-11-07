**Example**: 

```cpp
#include <pthread.h>

pthread_mutex_t mutex_handle;
int count;
int c;
pthread_t tid[8];

void increment(){
	pthread_mutex_lock(&mutex_handle);
    count = count + 1;
	pthread_mutex_unlock(&mutex_handle);
}

void get(){
    int c;
    pthread_mutex_lock(&mutex_handle);
	c = count;
	printf("%d", c);
    pthread_mutex_unlock(&mutex_handle);
}

int main(){
    int i = 0;
    while (i < 4){
        pthread_create(&(tid[i]), NULL, &increment, NULL);
        i++;
    }
    pthread_join(tid[0], NULL);
    pthread_join(tid[1], NULL);
    pthread_join(tid[2], NULL);
    pthread_join(tid[3], NULL);
    
    while (i < 8){
        pthread_create(&(tid[i]), NULL, &get, NULL);
        i++;
    }
    pthread_join(tid[4], NULL);
    pthread_join(tid[5], NULL);
    pthread_join(tid[6], NULL);
    pthread_join(tid[7], NULL);
}
```

Problem: Race condition if the mutex lock function is not successful, i.e. return value != 0. It can be fixed as follows:

```cpp
#include <pthread.h>

pthread_mutex_t mutex_handle;
int count;
int c;
pthread_t tid[8];

void increment(){
    int result;

    result = pthread_mutex_lock(&mutex_handle);
    if (0 != result)
        pthread_exit(NULL);
        
    count = count + 1;
	pthread_mutex_unlock(&mutex_handle);
}

void get(){
    int c;
    result = pthread_mutex_lock(&mutex_handle);
    if (0 != result)
        pthread_exit(NULL);
	c = count;
	printf("%d", c);
    pthread_mutex_unlock(&mutex_handle);
}

int main(){
    int i = 0;
    int ret = 0;
    
    while (i < 4){
        error = pthread_create(&(tid[i]), NULL, &increment, NULL);
        if (error != 0)
            printf("\nThread can't be created : [%s]", strerror(error));
        i++;
    }
    pthread_join(tid[0], NULL);
    pthread_join(tid[1], NULL);
    pthread_join(tid[2], NULL);
    pthread_join(tid[3], NULL);
    
    while (i < 8){
        error = pthread_create(&(tid[i]), NULL, &get, NULL);
        if (error != 0)
            printf("\nThread can't be created : [%s]", strerror(error));
        i++;
    }
    pthread_join(tid[4], NULL);
    pthread_join(tid[5], NULL);
    pthread_join(tid[6], NULL);
    pthread_join(tid[7], NULL);
}
```


- Real-world example: CVE-2021-38171(unchecked return value against ffmpeg).