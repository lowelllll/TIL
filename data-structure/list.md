# List

### Array

인덱스가 중요. (위치)

### List

인덱스보다는 <b>데이터가 저장되어있는 순서</b>를 더 중요하게 여김.



## List의 기능

- 처음,끝,중간에 엘리먼트를 추가/삭제할 수 있는 기능
- 리스트에 데이터가 있는지 체크
- 리스트의 모든 데이터에 접근하는 기능

등



Java에서는 배열과리스트를 모두 지원하는데, ArrayList와 LinkedList를 지원함.

### ArrayList

- 추가/삭제 느림
- 인덱스 조회 빠름

### LinkedList

- 추가/삭제 빠름
- 인덱스 조회 느림



## ArrayList

> 배열로 만든 리스트

- 추가/삭제가 느리고 조회가 빠름

  인덱스를 이용해 가져오는 것이 굉장히 빠름.

### 사용법

```java
import java.util.ArrayList;
import java.util.Iterator;

public class ArrayListEx {
	public static void main(String args[]) {
		ArrayList<Integer> numbers = new ArrayList<Integer>(); // ArrayList 객체 생성
		numbers.add(10); // 리스트에 요소 추가
		numbers.add(20);
		numbers.add(30);
		numbers.add(40); 

		System.out.println(numbers); // 리스트 전체 출력
		
		numbers.add(1,50); // 1인덱스에 50을 추가함.
		
		System.out.println(numbers); // [10,50,20,30,40]
		
		numbers.remove(2);
		
		System.out.println(numbers); // [10,50,30,40]
		
		System.out.println(numbers.get(1)); // 50
		System.out.println(numbers.size()); // 리스트의 길이를 출력 4
		
		// 반복문 사용하기
		Iterator<Integer> it = numbers.iterator(); // iterator 초기화. 원래 Iterator의 타입 object
	
		while(it.hasNext()) { // 다음 요소가 있으면
			int value = it.next();
		}
		
		for(int value: numbers) { // for each문을 사용한 요소 출력
			System.out.println(value);
		}
		
		for(int i=0; i<numbers.size(); i++) {
			System.out.println(numbers.get(i));
		}
	}
}
```



## refer

자바의 정석