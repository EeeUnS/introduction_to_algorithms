
//not my code
```
#include <iostream>
#include<utility>

#include <cstdio>
using namespace std;

int b[] = { 12,10 ,43,43 ,23,123,56,45,123 ,56,56,98,45,123,56,98,41,90,24 ,45 };//,-78,45,
 /**
   * @data  정수 배열
   * @size  data의 정수들의 개수
   * @p  숫자의 최대 자리수
   * @k  기수(10진법을 사용한다면 10)

   */
void rxSort(int* data, int size, int p, int k) 
{
	int* counts, // 특정 자리에서 숫자들의 카운트
		* temp; // 정렬된 배열을 담을 임시 장소
	int index, pval, i, j, n;
	// 메모리 할당
	if ((counts = (int*)malloc(k * sizeof(int))) == NULL)
		return;
	if ((temp = (int*)malloc(size * sizeof(int))) == NULL)
		return;
	for (n = 0; n < p; n++) { // 1의 자리, 10의자리, 100의 자리 순으로 진행
		for (i = 0; i < k; i++)
			counts[i] = 0; // 초기화
		   // 위치값 계산.
		  // n:0 => 1,  1 => 10, 2 => 100
		pval = (int)pow((double)k, (double)n);
		// 각 숫자의 발생횟수를 센다.
		for (j = 0; j < size; j++) 
		{
			// 253이라는 숫자라면
			// n:0 => 3,  1 => 5, 2 => 2
			index = (int)(data[j] / pval) % k;
			counts[index] = counts[index] + 1;
		}
		// 카운트 누적합을 구한다. 계수정렬을 위해서.
		for (i = 1; i < k; i++) 
		{
			counts[i] = counts[i] + counts[i - 1];
		}
		// 카운트를 사용해 각 항목의 위치를 결정한다.
		// 계수정렬 방식
		for (j = size - 1; j >= 0; j--) 
		{ // 뒤에서부터 시작
			index = (int)(data[j] / pval) % k;
			temp[counts[index] - 1] = data[j];
			counts[index] = counts[index] - 1; // 해당 숫자 카운트를 1 감소
		}
		// 임시 데이터 복사
		memcpy(data, temp, size * sizeof(int));
	}
	free(counts);
	free(temp);
}


void COUNTING_SORT(int A[], int n ,int k)//A의 수 n , k : 숫자범위
{
	int* C = new int[k];
	int* B = new int[n];
	for (int i = 0; i < k; i++)
	{
		C[i] = 0;
	}
	for (int j = 0; j < n; j++)
	{
		C[A[j]] = C[A[j]] + 1;
	}
	for (int i = 1; i < k; i++)
	{
		C[i] = C[i] + C[i - 1];
	}

	for (int j = n-1; j >= 0; j--)
	{
		B[C[A[j]]-1] = A[j];
		C[A[j]] = C[A[j]] - 1;
	}

	for (int i = 0; i < n; i++)
	{
		A[i] = B[i];
	}
	delete[] C;
	delete[] B;
}

void print(int A[], int n)
{
	for (int i = 0; i < n; i++)
	{
		cout << A[i] << ' ';
	}
	cout << '\n';
}

int main()//­
{
	int num = sizeof(b) / sizeof(int);
	print(b, num);

	COUNTING_SORT(b, num, 150);

	print(b, num);
	return 0;
}

```