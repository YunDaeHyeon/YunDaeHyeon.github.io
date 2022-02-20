---
layout: post
title: "[Java, 자료구조] Stack 구현하기(2/2) - Stack 구현"
data:   2022-02-19 17:40:00+0900
categories: jekyll update
tags: [java, data structure]
---

**해당 게시글은 [[Java, 자료구조] Stack 구현하기(1/2) - Stack Interface](https://daegom.com/main/datastructure-post5/)와 이어진다.**  
해당 포스팅은 자료구조 중 하나인 **Stack**에 대해 이해하기 위해 스터디 목적으로 직접 Stack을 구현해보는 포스팅이다. Java에서 지원하는 Stack 클래스에 대한 사용법이 아니다. 실습을 진행할 때 사용된 코드는 모두 [Stranger's Lab](https://st-lab.tistory.com/174?category=856997)님의 게시글과 설명을 참고로 하여 구현하였다.

# Stack 클래스 구현하기
**Stack 클래스를 생성한 뒤 StackInterface를 implements 시킨다.**  
*또한, StackInterface는 \<E\>를 사용하는 제네릭 인터페이스 이기에 Stack 클래스도 제네릭 클래스로 명시한다.*

## 1. 생성자와 변수 선언

```java
    private static final int DEFAULT_CAPACITY = 10;
    private static final Object[] EMPTY_ARRAY = {};
    private Object[] array;
    private int size;
```
`DEFAULT_CAPACITY`와 `EMPTY_ARRAY`는 모두 상수로 선언하여 값을 고정시킨다.  
  
**[변수 설명]**  
`DEFAULT_CAPACITY` : 스택의 Default 크기 설정  
`EMPTY_ARRAY` : 아무것도 없는 빈 Object 배열 선언  
`array` : 넘어오는 스택의 요소들을 담을 배열  
`size` : 배열에 담겨있는 요소의 개수 (크기 X)  

```java
    public Stack() { // 1. 초기 공간을 할당하지 않은 생성자
        this.array = EMPTY_ARRAY;
        this.size = 0;
    }

    public Stack(int capacity) { // 2. 초기 공간을 할당한 생성자
        this.array = new Object[capacity];
        this.size = 0;
    }
```
1. 초기 공간을 할당하지 않은 생성자  
```java
Stack<Integer> stack = new Stack<>();
```
위 코드처럼 스택의 크기를 할당하지 않았기에 `array`에 비어있는 Object 배열 `EMPTY_ARRAY`를 대입하여 array를 빈 배열로 초기화 시킨다. 
그 뒤 `size`에 0을 할당하여 스택에 있는 요소의 개수를 0으로 만든다.  
  
2. 초기 공간을 할당한 생성자  
```java
Stack<Integer> stack = new Stack<>(20);
```
위 코드는 스택의 크기를 할당하였기에 위 생성자에서 매개변수 `capacity`만큼의 Object 배열을 생성하여 array 배열에 대입한다. 위와 같이 `size`에 
0을 할당하여 스택에 있는 요소의 개수를 0으로 만든다.  

## 동적할당 구현 - resize()
들어오는 데이터의 개수에 따라 배열의 크기를 최적화 시켜야 하기에 동적할당을 수행하기 위한 메소드 `resize()`를 구현한다.  
```java
    private void resize() {
        if (Arrays.equals(array, EMPTY_ARRAY)) {
            array = new Object[DEFAULT_CAPACITY];
            return;
        }

        int arrayCapacity = array.length;
        if (size == DEFAULT_CAPACITY) {
            int newSize = arrayCapacity * 2; 
            array = Arrays.copyOf(array, newSize);
            return;
        }
        if (size < (arrayCapacity / 2)) {
            int newSize = arrayCapacity / 2;
            array = Arrays.copyOf(array, Math.max(DEFAULT_CAPACITY, newSize));
            return;
        }
    }
```
1. if (Arrays.equals(array, EMPTY_ARRAY))  
 `Arrays.equals()`는 두 개의 배열을 비교하여 같으면 true를 반환한다. `EMPTY_ARRAY`은 빈 Object 배열이기에 만약 **array == 0일 때. 즉, 배열 array가 비어있으면** 위에서 상수로 설정한 기본 스택의 크기 `DEFAULT_CAPACITY`만큼 새로운 Object 배열을 생성하여 array 배열에 대입한다.  
2. int arrayCapacity = array.length;  
 해당 메소드가 실행될 시점의 배열의 크기(스택의 크기, 요소의 개수가 아니다!!)를 구한다.  
3. if (size == DEFAULT_CAPACITY)  
 `Arrays.copyOf()`는 배열을 복사하는 의미이다. `newSize`에 기존 배열의 크기를 2배 늘린 값을 대입하여 그 값 만큼 크기를 늘린 배열을 기존 배열 `array`에 복사한다. 
**즉, `size(스택에 있는 요소의 개수)`와 `(DEFAULT_CAPACITY)기본으로 제공되는 배열의 크기`가 같으면 해당 배열(스택)의 크기를 2배 늘린다.**  
4. if (size < (arrayCapacity / 2))  
 해당 조건문은 **`size(스택에 있는 요소의 개수)`가 현재 배열의 크기의 절반보다 없다면**의 의미이다. 스택의 요소가 크기보다 훨씬 적으므로 사용하지 않는 배열의 공간을 줄일 필요가 있다. 따라서 `newSize`에 기존 배열의 크기를 2로 나눈 값을 대입하여 그 값 만큼 크기를 줄인 배열을 기존 배열 `array`에 복사한다.  
 이때 사용된 `Math.max(value1, value2)`는 **두 개의 인자 중 큰 값을 리턴하며 배열의 크기를 줄일 때 `(DEFAULT_CAPACITY)기본으로 제공되는 배열의 크기`보다 작게 설정되는 경우를 방지하기 위한 코드이다.**  
  
## push 구현

```java
    @Override
    public E push(E item) {
        if(size == array.length) resize();
        array[size] = item;
        size++;
        return item;
    }
```
`if(size==array.length) resize();`의 의미는 `push()`를 호출한 시점에 스택이 가득 찼다면(스택의 개수와 배열의 크기가 같다면) 스택의 크기를 동적으로 늘려줘야할 필요가 있기에 `resize()`메소드를 호출하는 의미이다.  
또한 **push는 스택에서 스택의 맨 위(TOP)에 요소를 차곡차곡 쌓는 원리이기에 스택(array)에서 마지막 요소의 위치(array[size])에 새로운 `item`을 push한다.**  
또, push를 함으로써 스택의 요소가 1개 늘었으니 후위증감연산자 `++`를 사용하여 size의 크기를 1 늘린다.  

## pop 구현

```java
    @Override
    public E pop() {
        if(size == 0) throw new EmptyStackException();

        @SuppressWarnings("unchecked")
        E obj = (E) array[size-1];
        array[size-1] = null;
        size--;
        resize();
        return obj;
    }
```
`if(size == 0)`은 만약, 스택이 비어있을 때 `EmptyStackException`예외를 뿌려주는 역할을 한다. **스택에서 pop은 마지막 요소(TOP)을 제거하기에 스택에 아무런 값이 없는데 값을 제거하려고 하면 오류가 발생하므로 그에 따른 예외처리를 출력해준다.**  
`E obj = (E) array[size-1];`는 스택의 마지막 값을 obj에 임시로 저장하는 역할을 수행하며 `array[size-1] = null;`을 통해 스택의 마지막 값을 null로 초기화 시켜 값을 제거한다.  
이때, `@SuppressWarnings("unchecked")`의 역할은 **타입 안전성 경고를 무시한다는 의미**이다. 아래의 코드를 보자.
  
```java
E obj = (E) array[size-1]
```
**배열 array는 Object 배열이다. 즉, Object 배열 `array`를 `제네릭 E 타입`으로 캐스팅(변환)을 시켜 `E obj`에 대입을 한다는 의미이다.** Object에서 E로 캐스팅이 진행될 때 해당 과정에서 변환할 수 없는 타입이 변환이 되어 `ClassCastException`이 발생할 수 있다는 경고를 뿌린다. 하지만, 위의 코드에서는 push 메소드가 받아들이는 타입은 `E 타입`으로 받아들이기에 안정성이 보장된다. 따라서 `@SuppressWarnings("unchecked")`를 통해 해당 경고를 무시한다는 의미이다.  

## peak 구현

```java
    @SuppressWarnings("unchecked")
    @Override
    public E peak() {
        if(size == 0)
            throw new EmptyStackException();
        return (E) array[size-1];
    }
```
**스택에서의 peak는 마지막 요소(TOP)을 제거하는 것이 아닌 반환하는 것이다. 따라서 TOP을 제거하는 과정만 지우면 된다.**  
위와 마찬가지로 스택이 비어있는 경우를 방지하기 위해 `EmptyStackException`처리를 해준다. 그 뒤 **size-1 위치에 있는 Object 배열 array를 E 타입으로 캐스팅하여 반환한다.** 이 또한 마찬가지로 **Object에서 E로 형변환이 수행되기에 `SuppressWarnings` 어노테이션을 명시한다.**  
**참고로, `array[size-1]`에서 배열에 있는 마지막 요소의 인덱스 값은 배열의 크기보다 1 작다.**  

## search 구현

```java
    @Override
    public int search(E value) {
        for(int i = size - 1; i >= 0 ; i--){
            if(array[i].equals(value)){
                return size - i;
            }
        }
        return -1;
    }
```
**스택에서의 search는 스택의 마지막 요소(TOP)에서 `value`까지 얼마나 떨어져 있는지에 대한 위치 값을 반환한다.** 이떄, 0이 아닌 1부터 값이 계산된다.  
배열의 크기가 아닌, **스택 요소의 개수만큼 반복문을 수행시켜 `equals(value)`를 통해 마지막 요소(TOP)부터 `value`까지의 값을 `size-i`를 통해 반환시킨다. 일치하는 데이터가 없는 경우 -1를 반환시킨다.  

## size 구현

```java
    @Override
    public int size() {
        return size;
    }
```
**스택에서의 size는 스택에 있는 요소의 개수를 출력해준다. (스택의 크기가 아니다!!)**  
간단하게 해당 메소드가 호출될 때 스택의 요소의 개수를 나타내는 `size`변수를 반환한다.

## clear 구현

```java
    @Override
    public void clear() { // clear()는 현재 스택의 모든 요소를 초기화한다.
        for(int i = 0 ; i < size; i++)
            array[i] = null; // 현재 스택에 있는 요소를 모두 null로 초기화 한다.
        size = 0; // 요소의 개수를 0으로 초기화
        resize(); // 스택의 크기를 다시 제어
    }
```
**스택에서 clear는 스택에 있는 모든 요소를 초기화하는 역할을 수행한다.** 따라서 현재 스택에 있는 요소의 개수만큼 반복문을 수행하여 배열(스택, array)에 있는 모든 값을 null로 초기화 시킨다. 스택에 있는 요소가 모두 null 처리되었기에 `size(스택에 있는 요소의 개수)`를 0으로 초기화시킨 뒤 `resize`메소드를 호출하여 배열(스택, array)의 크기를 제어한다.  

## empty 구현

```java
    @Override
    public boolean empty() {
        return size == 0;
    }
```
**스택에서 empty는 스택에 요소가 남아있는지, 없는지 판별하는 역할을 수행한다.** 단순하게 `size == 0`을 통하여 스택에 요소가 하나도 없다면 `true`를, 하나라도 존재한다면 `false`를 반환한다.

## 전체 코드 (주석 X)

```java
import java.util.Arrays;
import java.util.EmptyStackException;

public class Stack<E> implements StackInterface<E> {
    private static final int DEFAULT_CAPACITY = 10;
    private static final Object[] EMPTY_ARRAY = {};
    private Object[] array;
    private int size;

    public Stack() {
        this.array = EMPTY_ARRAY;
        this.size = 0;
    }

    public Stack(int capacity) {
        this.array = new Object[capacity];
        this.size = 0;
    }

    private void resize() {
        if (Arrays.equals(array, EMPTY_ARRAY)) {
            array = new Object[DEFAULT_CAPACITY];
            return;
        }

        int arrayCapacity = array.length;
        if (size == DEFAULT_CAPACITY) {
            int newSize = arrayCapacity * 2; 
            array = Arrays.copyOf(array, newSize);
            return;
        }
        if (size < (arrayCapacity / 2)) {
            int newSize = arrayCapacity / 2;
            array = Arrays.copyOf(array, Math.max(DEFAULT_CAPACITY, newSize));
            return;
        }
    }

    @Override
    public E push(E item) {
        if(size == array.length) resize();
        array[size] = item;
        size++;
        return item;
    }

    @Override
    public E pop() {
        if(size == 0) throw new EmptyStackException();

        @SuppressWarnings("unchecked")
        E obj = (E) array[size-1];
        array[size-1] = null;
        size--;
        resize();
        return obj;
    }

    @SuppressWarnings("unchecked")
    @Override
    public E peak() {
        if(size == 0)
            throw new EmptyStackException();
        return (E) array[size-1];
    }

    @Override
    public int search(E value) {
        for(int i = size - 1; i >= 0 ; i--){
            if(array[i].equals(value)){
                return size - i;
            }
        }
        return -1;
    }

    @Override
    public int size() {
        return size;
    }

    @Override
    public void clear() {
        for(int i = 0 ; i < size; i++)
            array[i] = null;
        size = 0;
        resize();
    }

    @Override
    public boolean empty() {
        return size == 0;
    }
}

```

### 전체 코드 (주석 O)

```java
package stack;

import java.util.Arrays;
import java.util.EmptyStackException;

public class Stack<E> implements StackInterface<E> {
    private static final int DEFAULT_CAPACITY = 10; // 최소 크기
    private static final Object[] EMPTY_ARRAY = {}; // 빈 배열 선언
    private Object[] array; // 요소를 담을 배열 선언
    /*
    Object는 모든 자바 클래스의 최상위 클래스이다.
    Stack을 사용할 때 넘어오는 클래스가 Primitive 혹은 Wrapper일 수 있기에 Object 배열 선언
     */
    private int size; // 요소의 개수 (배열의 크기가 아닌 요소의 개수)

    // 초기 공간을 할당하지 않았을 경우의 생성자
    public Stack() {
        this.array = EMPTY_ARRAY; // 초기 공간이 없는 빈 배열을 array에 대입
        this.size = 0;  // 요소의 개수
    }

    // 초기 공간을 할당하였을 경우의 생성자
    public Stack(int capacity) {
        this.array = new Object[capacity]; // 파라미터의 값 만큼 Object[] array의 크기 할당
        this.size = 0; // 요소의 개수
    }

    // 기본적으로 스택은 Vector 클래스를 상속받기에 ArrayList와 유사하다.
    // 데이터를 효율적으로 관리하기 위해 최적화를 틈틈히 수행해야 할 필요가 있다.
    // 요소의 개수가 배열에 얼마나 차 있는지 확인하고 적절한 크기에 맞춰 배열의 크기를 컨트롤한다. (동적할당)
    private void resize() {

        // util의 Arrays.equals(array1, array2)를 사용하여 두 배열이 같은지 비교한다.
        // EMPTY_ARRAY의 경우 static final로 선언되어 비어있는 상수 배열이다.
        // resize()를 불러올 때 스택이 비어있을 경우
        if (Arrays.equals(array, EMPTY_ARRAY)) {
            array = new Object[DEFAULT_CAPACITY]; // 스택이 비어있으므로 최소 크기를 지정
            return;
        }

        int arrayCapacity = array.length; // 현재 스택의 크기 저장
        // resize()를 불러올 때 스택이 가득 차있을 경우
        if (size == DEFAULT_CAPACITY) { // 스택에 있는 요소의 개수가 기본 배열의 크기(10)보다 크면
            int newSize = arrayCapacity * 2; // 스택이 가득 차있으므로 현재 배열의 크기를 두 배로 늘린다.

            // Arrays.copyOf(array1, length)를 사용하여 용량을 두 배로 늘린 배열을 기존의 배열에 복사한다.
            array = Arrays.copyOf(array, newSize);
            return;
        }

        // resize()를 불러올 때 요소가 현재 스택 크기의 절반보다 작을 경우
        if (size < (arrayCapacity / 2)) {
            int newSize = arrayCapacity / 2;

            // Math.max(value1, value2)는 두 개의 인자 중 큰 값을 리턴한다.
            // 스택의 크기가 최소 크기(10)보다 작게 설정되는 것을 방지하기 위함.
            array = Arrays.copyOf(array, Math.max(DEFAULT_CAPACITY, newSize));
            return;
        }
    }

    @Override
    public E push(E item) {
        if(size == array.length) // push()를 수행했을 때 스택의 용량이 꽉 찬 상태라면
            resize();
        array[size] = item; // Stack은 Last in First Out 이기에 push를 수행하면 top에 요소를 추가한다.
        size++; // 요소를 추가한 뒤 요소의 개수를 늘린다.
        return item;
    }

    @Override
    public E pop() {
        if(size == 0) // 만약 스택에 삭제할 요소가 존재하지 않는다면 스택이 비어있는 의미이기에 예외를 발생시킨다.
            throw new EmptyStackException(); // EmptyStackException은 스택이 비어있는 것을 나타내는 예외이다.

        @SuppressWarnings("unchecked")
        E obj = (E) array[size-1]; // 삭제될 요소를 반환하기 위한 임시 변수 선언
        array[size-1] = null; // 스택 top의 요소 삭제
        size--; // 요소의 개수 줄이기
        resize(); // 스택 재할당
        return obj;
    }

    @SuppressWarnings("unchecked")
    @Override
    public E peak() {
        if(size == 0)
            throw new EmptyStackException();
        return (E) array[size-1]; // 배열에서 마지막 원소의 인덱스는 개수보다 1개 작다.
    }

    @Override
    public int search(E value) { // search는 찾으려는 데이터가 top으로부터 얼마나 떨어져 있는지 찾기 위한 메소드
        for(int i = size - 1; i >= 0 ; i--){ // size-1은 요소의 top의 인덱스를 의미
            if(array[i].equals(value)){
                return size - i;
            }
        }
        return -1; // 일치하는 데이터가 없는 경우
    }

    @Override
    public int size() { // size()는 현재 스택에 있는 요소의 개수를 반환한다.
        return size;
    }

    @Override
    public void clear() { // clear()는 현재 스택의 모든 요소를 초기화한다.
        for(int i = 0 ; i < size; i++)
            array[i] = null; // 현재 스택에 있는 요소를 모두 null로 초기화 한다.
        size = 0; // 요소의 개수를 0으로 초기화
        resize(); // 스택의 크기를 다시 제어
    }

    @Override
    public boolean empty() { // 스택이 비어있는지에 대한 여부를 판별
        return size == 0;
        // size(요소의 개수)가 0이라면 true(1)를 반환, 아니라면 false(0)를 반환
    }
}

```
  
*자바는 배워도 배워도 끝이 없는 것 같다. 하지만 스택을 직접 구현해보니 바로바로 이해가 되어 무척이나 재미있다.*
  


---  
[참고 및 리소스 출처]  
[Stranger's LAB](https://st-lab.tistory.com/174?category=856997)  
[코딩팩토리](https://coding-factory.tistory.com/601)  