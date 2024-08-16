# BOM-BEStudy
구름톤 유니브 3기 Back-end 파트

<!--사용방법-->
# Markdown 문법
## 1. 헤더
```
# 가장 큰 제목
## 두번째 큰 제목
### 세번째 큰 제목
#### 네번째 큰 제목
##### 다섯번째 큰 제목
###### 여섯번째 큰 제목
```

---

## 2. 목록
### 순서가 있는 목록은 숫자와 점을 사용합니다.

```
1. 첫번째
2. 두번째
3. 세번째
```
<img width="300" alt="image" src="https://user-images.githubusercontent.com/88806404/195787105-195f7f10-f82d-4bef-8d71-487a79dfc7a6.png">
<br><br>

### 순서가 없는 목록(*,+,-)

```
* 빨강
    * 녹색
        * 파랑

+ 빨강
    + 녹색
        + 파랑

- 빨강
    - 녹색
        - 파랑
```
<img width="300" height=200 alt="image" src="https://user-images.githubusercontent.com/88806404/195787502-fd904be7-0472-48d2-98d3-ee1621435e7e.png">
<br><br>
혼합하여 사용도 가능합니다.

```
* 1단계
    - 2단계
        + 3단계
            + 4단계
```
<img width="300" height=200 alt="image" src="https://user-images.githubusercontent.com/88806404/201568082-7b0f8a9c-ed6e-4f23-baf3-c65022538bd4.png"><br>

---


## 3. 코드
4개의 공백 또는 하나의 탭으로 시작하고 들여쓰지 않은 행을 만날때 까지 변환이 계속됩니다.<br>
### 3-1. 들여쓰기

    this is sample code
        this is sample code
<br>

### 3-2. 코드 블럭
코드 블럭은 다음과 같이 2가지 방식으로 사용합니다.
+ \<pre>\<code>{code}\</code>\</pre><br><br>
ex)<br>
    ```
    <pre>
    <code>
    public class Sample {
        public static void main(String[] args) {
            System.out.println("Hello, World");
        }
    }
    </code>
    </pre>
    ```
    적용
    ```
    public class Sample {
        public static void main(String[] args) {
            System.out.println("Hello, World");
        }
    }
    ```
<br>

+ 코드 블럭 (```) 사용
    ```
       ```
        public class Sample {
        public static void main(String[] args) {
            System.out.println("Hello, World");
            }
        }
        ```
    ```
    적용
    ```
    
    public class Sample {
    public static void main(String[] args) {
        System.out.println("Hello, World");
        }
    }
    ```
깃허브에서는 코드블럭코드 (```) 시작점에 사용하는 언어를 선업하여 <b>문법강조(Syntax highlighting)</b>이 가능합니다.
```
    ```java
        public class Sample {
        public static void main(String[] args) {
            System.out.println("Hello, World");
            }
        }
    ```
```
결과
```java
    public class Sample {
    public static void main(String[] args) {
        System.out.println("Hello, World");
        }
    }
```

---


<br>

## 4. 수평선
아래에 있는 것들은 모두 줄을 만듭니다. 마크다운 문서를 미리보기로 출력할 때 페이지 나누기 용도로 많이 사용합니다.
<br>
```
* * *

***

*****

- - -

------------
```
적용
* * *
***
*****
- - -
-------------

<br>

## 5. 링크
+ 참조링크
```
[link keyword][id]
[id]: URL "Optinal Title here"

// code
Link: [Google](googlelink)

[googlelink]: https://google.com "Go google"
```
Link: [Google](https://google.com)<br>

+ 외부 링크
```
사용문법: [Title](link)
적용예시: [Google](https://www.google.com, "google link)
```
Link: [Google](https://www.google.com)
+ 자동연결
```
일반적인 URL 혹은 이메일주소인 경우 적절한 형식으로 링크를 형성한다.

* 외부링크: <https://www.naver.com>
* 이메일링크: <https://www.github.com/Yu-Jeseung>
```
* 외부링크: <https://www.naver.com>
* 이메일링크: <https://www.github.com/Yu-Jeseung>

---


## 6. 강조
```
*single asterisks*
_single underscores_

**double asterisks**
__double underscores__

~~cancelline~~
```
+ *single asterisks*<br>
+ _single underscores_

+ **double asterisks**<br>
+ __double underscores__

+ ~~cancelline~~

문장 중간에 사용할 경우에는 **띄어쓰기** 를 사용하는 것이 좋다.<br>

---

## 7. 이미지
```
![Alt text](/Document/사진/뿌뿌.png)
![Alt text](/Document/사진/뿌뿌.pngg "Optional title")
```

<img alt="뿌뿌" src="https://user-images.githubusercontent.com/88806404/201942186-0ba276a8-b575-4fec-90ca-ec5d922576ae.png"><br>

사이즈 조절 기능은 없기 때문에 \<img width="" height="">\</img>를 사용한다.

```
<img src="/Document/사진/뿌뿌.png" width="450px" height="300px" title="px(픽셀) 크기 설정" alt="뿌뿌"></img><br/>
<img src="/Document/사진/뿌뿌.png" width="40%" height="30%" title="px(픽셀) 크기 설정" alt="뿌뿌"></img>
```
<img alt="뿌뿌" width="450px" height="300px" src="https://user-images.githubusercontent.com/88806404/201942186-0ba276a8-b575-4fec-90ca-ec5d922576ae.png">


---


## 8. 줄바꿈
3칸이상 띄어쓰기를 하면 줄이 바뀐다.
```
* 줄 바꿈을 하기 위해서는 문장 마지막에서 3칸 이상을 띄어쓰기 해야 한다.
이렇게

* 줄 바꿈을 하기 위해서는 문장 마지막에서 3칸 이상을 띄어쓰기 해야 한다.(   )
이렇게
```
* 줄 바꿈을 하기 위해서는 문장 마지막에서 3칸 이상을 띄어쓰기 해야 한다.
이렇게

* 줄 바꿈을 하기 위해서는 문장 마지막에서 3칸 이상을 띄어쓰기 해야 한다.   
이렇게

---

## 9. 테이블 사용
+ 헤더와 셀을 구분할 때 **3개 이상의 -(hyphen/dash)기호**가 필요합니다.<br>
+ 헤더 셀을 구분하면서 **:(Colons) 기호로 셀(열/칸) 안에 내용을 정렬**할 수 있습니다.
+ 가장 좌측과 가장 우측에 있는 |(vertical bar) 기호는 생략 가능합니다.
```
테이블 생성
 
헤더1|헤더2|헤더3|헤더4
---|---|---|---
셀1|셀2|셀3|셀4
셀5|셀6|셀7|셀8
셀9|셀10|셀11|셀12
```
헤더1|헤더2|헤더3|헤더4
---|---|---|---
셀1|셀2|셀3|셀4
셀5|셀6|셀7|셀8
셀9|셀10|셀11|셀12

<br>


---
---
## 출처
+ [마크다운 (Markdown)](https://gist.github.com/ihoneymon/652be052a0727ad59601)
<br>

+ [Dev Scroll](https://inpa.tistory.com/entry/MarkDown-📚-마크다운-문법-💯-정리)