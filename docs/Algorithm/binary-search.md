# Binary Search

## 이진 탐색 수행 과정

이진 탐색은 정렬된 데이터에서 사용할 수 있는 고속 탐색 알고리즘이다. 왜 고속인가? 탐색 범위를 절반으로 계속 줄여나가기 때문이다. 그래서 이름도 `이진`탐색이다.  
이진 탐색 과정은 아래와 같다.

1. 데이터 중앙의 요소를 찾는다.

2. 중앙의 요소와 찾는 값(target)을 비교한다.

3. 목표값이 중앙의 요소보다 작으면 탐색 범위를 중앙의 요소 기준으로 왼쪽 절반으로 이진 탐색을 수행한다. 아니라면 탐색 범위를 오른쪽 절반으로 이진 탐색을 수행한다.

4. 반복한다.

## 이진 탐색의 구현

```<C>
int BinarySearch( int arrays[], int size, int target ) {

	int left, right, mid;

	left = 0;
	right = size - 1;

	while (left <= right) {
		mid = left + (right - left) / 2;

		if (arrays[mid] == target) {
			return mid;
		}
		else if (arrays[mid] > target) {
			right = mid - 1;
		}
		else {
			left = mid + 1;
		}
	}

	// 못 찾음
	return -1;
}
```

기본적인 구현은 위와 같다. 이 때 가장 구현하기 어려운 점은 인덱스다. 특히 반복문의 종료조건이 직관적으로 느껴지지 않는다. 왜 등호가 들어가야할까?

우선 생각해볼 것이 mid = left + (right - left) / 2 를 구할 때 분자 값이 홀수라면 나머지를 버리게 된다. 즉 배열에 짝수개가 있을 때 mid는 가운데 2개의 요소 중 왼쪽을 택하게 된다는 것이다.

등호가 없을 때 어떤 일이 일어나는지 생각해보자.  
[2, 4, 6, 10, 16, 19, 22, 23, 30, 32] 에서 이분 탐색을 통해 10을 찾아보자.

반복 1:
left_idx = 0, right_idx = 9, mid_idx = 4
지금도 배열에 원소가 짝수 개라서 인덱스 4,5 중 왼쪽인 4가 mid_idx임을 확인할 수 있다.  
이제 target값인 10과 비교했을 때 mid에 위치한 값은 16이다. 따라서 범위를 절반으로 줄이고 왼쪽을 찾아야한다. right_idx이 3이된다.

반복 2:
left_idx = 0, right_idx = 3, mid_idx = 1
다시 배열이 짝수 개가 되었다. mid의 위치한 값은 4며 target보다 작기 때문에 범위를 오른쪽 절반으로 줄여야한다. left_idx는 2가된다.

반복 3:
left_idx = 2, right_idx = 3, mid_idx = 2
이제 배열에 원소 2개밖에 남지 않았다. mid에 위치한 값인 6은 target보다 작기 때문에 범위를 오른쪽 절반으로 줄여야한다. left_idx는 3이된다.  
이제 집중해야한다. left_idx와 right_idx가 같아졌다. 만약에 반복문에 등호 조건이 없다면 루프가 끝나게 된다. 즉 target은 인덱스 3에 위치하고 있는데 찾지 못하는 결과가 발생하게 된다.

결론적으로 헷갈리면 반복 3의 상태를 생각하면 된다. 원소가 2개인 배열에서 오른쪽에 target이 위치하는 경우를 생각해보자. [10, 20] target = 20
이런 극단적인 예시를 외운다면 이진 탐색을 구현할 때 루프 조건을 기억하기 쉽다.

## 이진 탐색 응용

이진 탐색을 통해 배열에 수가 있는지는 확인할 수 있다. 하지만 그 수가 여러 개 배열에 있는 경우 그 수가 몇 번 등장했는지는 알 수 없다.

어떤 수가 몇 번 등장했는지 알기 위해서는 이진 탐색을 응용해야한다.

예를들어 [2, 4, 6, 10, 10, 16, 16, 16, 30, 32]와 같은 배열이 있다고 하자.

예시를 통해 이진 탐색 응용을 일반화 해보자. 16은 3번 등장했다. 이 것을 구하는 방법을 요약해보면 16이 처음으로 등장하지 않는 위치(인덱스) 8에서 16이 첫번째로 등장하는 위치 5를 빼면 8 - 5로 3을 얻을 수 있다.

이 개념은 한 번도 등장하지 않는 원소, 오직 한 번 등장하는 원소, 여러 번 등장하는 원소등 모두 적용가능하다.

여기서 발상을 추가해보자. `특정 원소를 배열에 밀어넣고, 오름차순이 유지되게 하는 방법`을 생각해보자.

예를 들어 16을 밀어넣어보자. 5번, 6번, 7번, 8번 모두 밀어넣을 수 있다. `밀어넣을 수 있는 가장 큰 인덱스 값에서 가장 작은 인덱스 값을 뺀 것이 해당 원소의 개수이다.`

이제 `밀어넣는다`의 개념을 구체화할 필요가 있다. 밀어넣으면 원래 그 위치에 있던 수와 그 뒤에 있는 수들은 인덱스가 늘어나는 쪽으로 한 칸씩 늘어나게 된다.

결론적으로 밀어넣을 수 있는 가장 작은 인덱스와 가장 큰 인덱스를 구하면 해당 원소의 개수를 구할 수 있다.

한 번도 등장하지 않는 원소로 이 개념을 검증해보자. 예를 들어 11을 밀어넣을 수 있는 가장 작은 인덱스와 가장 큰 인덱스를 찾아보자. 가장 작은 인덱스는 5이며, 가장 큰 인덱스도 5이다. 다른 위치는 모두 배열의 오름차순이 깨지게된다. 5 - 5를 해서 0이나오고 11이 등장하지 않는다는 가정과도 모순되지 않는다.

자 이제 이진 탐색을 응용해서 배열에서 원소의 개수를 구하는 개념을 익혔으니 이를 구현해야한다.

구현은 2단계로 이루어진다.

1. 가장 작은 인덱스 구하기
2. 가장 큰 인덱스 구하기

각각을 구하는 코드를 구현하기 전 예시를 통해서 시뮬레이션 해보자. 16을 넣을 수 있는 가장 작은 인덱스를 구할 건데 우선 이 16이 들어갈 수 있는 후보 인덱스를 찾아보자.
0부터 10까지 총 11개다. `마지막에 10이라는 인덱스가 추가된 것을 유의하자`  
이제 이 후보를 이진 탐색으로 절반씩 줄여나가면서 가장 작은 인덱스(16이 들어갈 수 있는 구간에서 가장 왼쪽)을 찾아보자.

반복 1:  
arr = [2, 4, 6, 10, 10, 16, 16, 16, 30, 32]  
l = 0, r = 10, m = 5

arr[m] = 16이다. 해당 위치인 5에 16을 넣을 수 있는가? 넣을 수 있다. 이제 구간을 줄여야한다. 5에는 넣을 수 있으니까 이런 사실을 알 수 있다. `처음으로 16이 등장할 수 있는 인덱스 값은 최소 5보다 같거나 작다는 것이다.` 위의 예시의 경우 5에 16을 넣고 정확하게 처음으로 16이 등장하는 구간이 됬지만, 4나 3위치의 10대신 16이 존재하는 경우도 상상해볼 수 있다.  
이런 사실로 어떤 행동을 취해야할 까? 결국 처음으로 16이 등장할 수 있는 위치를 찾기 위해 구간을 좁혀나간다는 원래의 목적을 생각해보면 이 5라는 위치는 새로 정의하는 구간의 `상한선`이 된다. 즉 우리의 목표에 따르면 5보다 큰 위치에 대해서는 조사해볼 필요가 없다!

따라서 r = 5가 된다. 즉 이제 5부터 10까지 위치에 5를 넣을 수 있다. 따라서  
**결론적으로 arr[m] = target이면 r을 mid로 교체하면 된다.**

반복 2:  
l = 0, r = 5, m = 2

arr[2] = 6이다. arr[2] < target 인 상황이다. 여기서 우리는 무엇을 알 수 있을까? 인덱스 2에 target인 16을 넣을 수 있을까? 그럴 수 없다. 인덱스 2에 16을 넣게 되면 원래 6이 오른쪽으로 밀리고 오름차순 정렬이 깨지게된다. 따라서 우리는 2보다큰 3부터 16을 넣을 수 있는 후보라고 간주할 수 있다. 이제 2보다 같거나 작은 구간에 대해서 조사할 필요가 없게 된다.

따라서 l = 3이 된다. 이제 3부터 5까지 구간이 16을 넣을 수 있는 후보가 되었다.  
**결론적으로 arr[m] < target이면 l을 m + 1으로 교체하면 된다.**

반복 3:
l = 3, r = 5, m = 4

arr[4] = 10이다. arr[4] < target인 상황이다. 반복 2의 상황과 동일하다. 따라서 4 이하의 인덱스에 대해서 조사할 필요가 없다. 따라서 l = 5가 된다.  
이제 l = 5, r = 5의 상황이다. 더 이상 반복할 필요가 없다. 우리의 목적은 16이 들어갈 수 있는 가장 왼쪽의 위치를 찾는 것이라. l과 r이 같아져 하나의 위치만 남으면 그것이 16을 넣을 수 있는 가장 왼쪽 인덱스인 5이다. 실제로 넣어보면 그런 것을 확인할 수 있다.

위의 16의 예시에서는 arr[index] > target인 경우를 찾지 못했다. 따라서 10으로 한 번 더 시뮬레이션 해보자.

반복 1:  
arr = [2, 4, 6, 10, 10, 16, 16, 16, 30, 32]  
l = 0, r = 10, m = 5

arr[5] = 16이다. arr[5] > 10이기 때문에 바로 이 경우를 만났다. 우선 인덱스 5에 10을 넣으면 오름차순이 깨지지 않는다. 기존의 16이 오른쪽으로 한 칸 밀리기 때문이다. 하지만 5보다 인덱스 6은 어떨까? 인덱스 6에 10을 넣으면 인덱스 5에 16이 있기 때문에 오름차순이 깨지게된다. 따라서 우리는 6보다 큰 구간에 대해서는 16을 넣을 수 있는 후보를 찾을 수 없다는 결론을 내릴 수 있다. 따라서 이제 5 이하의 위치만 조사하면 된다.
따라서 r = 5가 된다.  
**결론적으로 arr[m] > target이면 r을 m으로 교체하면 된다.**

반복 2:
l = 0, r = 5, m = 2

arr[2] = 6이다. arr[2] < 10이다. 위에서 정리한대로 l = m + 1이 된다.

반복 3:
l = 3, r = 5, m = 4
arr[4] = 10이다. arr[4] = 10이다. 위에서 정리한대로 가장 왼쪽 인덱스를 찾는 목적에 따르면 r = m이다.

반복 4:
l = 3, r = 4, m = 3
arr[3] = 10이다. r = m이다.  
이제 r = ㅣ = 3이 되어 더 이상 반복할 필요가 없다. 위치 3이 10을 추가할 후보 중 가장 왼쪽이 되어, 10이 처음으로 등장하는(인덱스 0부터 읽는다고 할 때) 위치가 된다.

이를 코드로 구현해보자.

```<C>
int left_bound( int arrays[], int size, int target ) {

	int l, r, m;

	l = 0;
	r = size; // 기존 binarysearch와 다르게 가장 왼쪽 후보지는 size부터다.

	while (l < r) {
		m = l + (r - l) / 2;

		if (arrays[m] >= target) {
			r = m;
		}
		else {
			l = m + 1;
		}
	}

	// 넣을 수 있는 후보 중 가장 왼쪽 위치 반환
	return l;
}
```

이제 가장 큰 인덱스를 구하는 시뮬레이션을 해보자. target은 10이며, 배열은 그대로 [2, 4, 6, 10, 10, 16, 16, 16, 30, 32]을 사용하자.

처음 후보는 가장 작은 인덱스를 구할 때 처럼 동일하다. 0 부터 10까지다.

반복 1:
l = 0, r = 10, m = 5

arr[5] = 16이다. arr[5] > 10인 상황이다. 목적을 염두하면서 어떻게 구간을 줄일 것인가 생각해보자. 우선 인덱스 5에 10을 넣으면 오름차순이 깨지는가? 그렇지 않다. 기존의 16이 인덱스 6으로 이동하고 여전히 오름차순의 정렬은 유지된다. 그렇다면 이 보다 작은 4인 인덱스에는 10을 밀어 넣을 수 있을까? 인덱스 4에 10보다 작은 수가 있다면 불가능하다. 10보다 큰 수가 있다면 밀어 넣을 수 있다.(인덱스 4에는 16 이하의 수가 오게 된다.) 하지만 작은 수가 있는 지 큰 수가 있는지 확정할 방법이 없다. 따라서 이 사고 실험에서는 목적에 맞는 범위를 줄여나가는 방법을 생각하기가 어렵다. 그렇다면 인덱스 6에 밀어 넣을 수 있을까? 인덱스 6에 10을 넣을 수 없다. 왜냐하면 인덱스 6에 10을 넣는 순간 인덱스 5의 16 때문에 오름차순이 깨지게 된다. 따라서 이 사고 실험에서는 확실한 결론을 얻을 수 있다. 5보다 큰 인덱스에 대해서는 10을 넣을 가능성이 전혀 없다는 것이다. 따라서 우리의 목적에 따라서 인덱스 5는 `상한선`이 된다. 생각보다 결론이 빨리 나오는 느낌이긴 한데. 이제 10이 들어갈 후보 중 가장 오른쪽 인덱스를 찾는 우리의 입장에서 구간을 변경하는 동작은 r = m이다.  
**결론적으로 arr[m] > target이면 r = m이 된다.**

반복 2:
l = 0, r = 5, m = 2

arr[2] = 6이다. arr[2] < 10 인 상황인데, 다시 사고 실험을 해보자. 인덱스 2에 10이 들어갈 수 있을까? 불가능하다 10이 인덱스 2에 들어가면 기존의 6이 인덱스 3으로 밀려나 오름차순의 정렬이 깨지게 된다. 사고 실험 결과 확실한 결과가 나왔기 때문에 우리의 목적을 다시 생각해보자 결국 10이 들어갈 수 있는 가장 큰 인덱스를 찾는 것과 연관지어 생각해보면, 2 이하의 인덱스는 조사할 필요가 없다. 따라서 인덱스 3이 새로운 `하한선`이 된다. 그렇다면 인덱스 3을 대상으로 사고 실험을 해서 검증을 해보자. 인덱스 3에 10을 밀어 넣으면 어떻게 될까? 인덱스 3에 10보다 큰 수가 있냐 같거나 작은 수가 있냐에 따라 결과는 달라진다.(인덱스 3에는 6 이상의 수는 모두 올 수 있다.) 즉 결론이 안 나는 사고실험이다. 이렇게 3이 새로운 하한선이라는 것에 대한 검증은 완료되었고, 구간을 변경하는 동작은 l = m + 1이 된다.  
**결론적으로 arr[m] < target이면 l = m + 1이 된다.**  
`그리고 중요한 사고 실험 방법에 대한 교훈을 배울 수 있다. 결과과 확정되는 사고 실험에서 결론을 도출하는 식으로 증명의 흐름을 잡을 수 있다!!`

반복 3:
l = 3, r = 5, m = 4

arr[4] = 10이다. arr[4] == 10 인 상황이다. 다시 사고 실험을 해보자. 인덱스 4에 10을 넣으면 어떻게 될까? 일단 10을 넣어도 무방하다, 10을 넣게 되면 기존의 10이 인덱스 5로 밀리게 되고 이는 오름차순 정렬을 깨지 않기 때문이다. 여기서 잠정적으로 인덱스 4가 상한선 or 하한선이 될 것이라고 예상할 수 있다. 그렇다면 인덱스 3에는 넣어도 될까? 인덱스 3에 어떤 숫자가 있냐에 따라 달라진다. 일단 인덱스 3에 들어갈 수 있는 숫자는 10 이하의 수다. 따라서 10이면 넣을 수 있지만 10보다 작은 수가 있드면 밀어 넣을 수 없다. 10이 인덱스 3에 들어가고 그보다 작은 수가 인덱스 4로 밀려나 오름차순 정렬이 깨지기 때문이다. 아직도 확정 할 수 없다. 마지막 사고 실험을 해보자 인덱스 5에 10을 넣을 수 있을까? 인덱스 5에는 10 이상의 수가 들어간다. 따라서 반드시 넣을 수 있다. 10 이상의 수가 오른쪽으로 밀려나 인덱스 6으로 간다고 해도 오름차순은 깨지지 않기 때문이다. 와! 결과가 확정되어었다. 결국 인덱스 5는 `하한선`이 된다. 3 이하의 인덱스는 조사할 필요가 없다! 인덱스 4의 경우는 우리의 목적이 가장 오른쪽 인덱스를 찾는 것이기 때문에 5와 4중 굳이 4를 고를 필요가 없다. 5로 범위를 더 줄이는 것이 맞다.
따라서 구간을 줄이는 작업을 하면 l = m + 1을 수행하면 된다.  
`결론적으로 arr[m] == target 이면 l = m + 1이 된다.`  
이제 l과 r이 같아지고 반복이 끝났다. l = m = 5가 10이 들어갈 수 있는 가장 오른쪽 인덱스가 된다.

이를 코드로 구현해보자.

```<C>
int right_bound( int arrays[], int size, int target ) {

	int l, r, m;

	l = 0;
	r = size; // 기존 binarysearch와 다르게 가장 오른쪽 후보지는 size부터다.

	while (l < r) {
		m = l + (r - l) / 2;

		if (arrays[m] > target) {
			r = m;
		}
		else {
			l = m + 1;
		}
	}

	// 넣을 수 있는 후보 중 가장 오른쪽 위치 반환
	return r;
}
```