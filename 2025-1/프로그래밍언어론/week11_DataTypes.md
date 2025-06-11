# [프로그래밍언어론 11주차]

## Data Objects
데이터 객체(data objects)는 내가 정의하는 것과, OS가 정의하는 것으로 나눌 수 있다
- Programmer-defined: 내가 선언([declaration](#declaration))을 통해 명시적으로 만들고 statement를 통해 조작하는 경우
	- ex. variables: 내가 명시적으로 이름을 붙여 정의하고, 값을 저장할 수 있다
		- variable에 바인딩된 값은 달라질 수 있다
	- ex. constants: `const max = 10` 등으로 정의하며, 값은 변경할 수 없다
		- cf. literal: `10`처럼, 값 그 자체이다
- System-defined: 프로그램을 실행하기 위해 OS가 정의하는 경우로, 내가 직접 접근할 수는 없다
	- ex. run-time storage stacks, subprogram activation records, file buffers, free-space lists, etc

<br>

## Evolution of the data type concept
- 초창기: 특정한 구조 없이, 값으로만 구성되었다
	- 여러 조합을 묶은 자료형의 필요성이 제기되었을 것이다

- Pascal (1970년): data object의 구조와, 그곳에 넣을 수 있는 값을 묶어 하나의 type을 정의했다
- 1970년대 초반: data object(구조 + 값)와, 이에 사용되는 operation을 묶어 type을 정의했다
	- 의도하지 않은 방식으로 데이터 값이 변경되지 않도록 하여, 버그를 막고자 하는 요구가 있었을 것이다
- Abstract data type의 등장: data object와, 이에 사용되는 operation, 그리고 정의된 operation 외에는 값을 변경할 수 없도록 하는 encapsulation까지 포함하여 type을 정의하였다
	- 메소드를 하나하나 만들어야 하는 번거로움이 생겼을 것이다(이후 시간이 지나며 이 또한 해결되었다)
> ### Encapsulation
> : 특정 데이터(멤버변수)의 경우, 직접 사용하거나 변경하는 것이 허용되지 않도록 접근 제어자를 설정한다
> - 프로그램의 신뢰성, 가독성이 향상된다
> - 데이터 수정 시 관련 부분만 수정하면 되기에, 프로그램의 수정이 용이해진다
> ### cf. Information hiding
> 현재, 프로그램은 모듈이라는 작은 부분으로 세분화되며, 모듈은 헤더파일로 배포된다
> 필요한 모듈만 모아 하나의 프로그램을 만든다
> 이때 사용에 필요한 정보만 제공해도 충분하다
> 나는 built-in 함수를 사용만 하면 되지, 함수의 내부 동작을 알 필요가 없기 때문이다
> 따라서 이러한 정보는 제공되지 않도록 한다
> 이와 관련하여, 프로그래밍 언어는 추상화(abstraction)를 사용할 수 있도록 functional abstraction(추상 라이브러리), abstract data type(추상 자료형), control abstraction(기계어와 무관한 제어구조)을 제공한다
>
> <br>
>
> ps. 프로그램을 모듈로 세분화할 때, function 단위로 할 수도 있고, 데이터 단위로 할 수도 있다
> 이때 function 단위로 하는 경우, 각 모듈이 주자료구조의 detail을 알아야 하기에 불편하다
> 현재는 데이터 단위로 세분화하는 것이 더 선호되는 방식이다

## Data types
: data object와, 이를 생성하고 조작하는 operation의 묶음을 모아놓은 것이다

- data type 설계 시 고려할 요소들
	- 속성 ex. array라면 차원, array를 구성하는 자료형, 인덱스가 어디부터 시작하는지 등
	- 들어갈 수 있는 값
	- operation의 동작 결과

- data type 구현 시 고려할 요소들
	- 실행했을 때 data object를 어느 지역 공간에 할당할 것인지
	- operation의 로직 구현
### Scalar data type
: 기본적인 data object들로, 하나의 value를 갖는다(ex. Integer, real, character, Boolean, enumeration, pointer, etc)

#### scalar data type 설계 시 고려할 요소들
	- 속성(타입, 이름)
	- 들어갈 수 있는 값
	- operation의 동작 결과(input에 대한 result가 있어야 한다)
#### scalar data type 구현 시 고려할 요소들
	- operation 구현 방식
		- 하드웨어 operation 이용
		- built-in 함수 사용
		- in-line code 사용(프로그램에 operation 코드가 복사되어 실행 중 호출되지 않아 효율적이지만, 콜에 따른 디버깅이 불가하다)
#### storage representation
	- 하드웨어를 이용하는 방법: 하드웨어 연산을 이용할 수 있어 효율적이었고, 초창기 C, Fortran, Pascal 등에서 사용되었다
	- data object의 속성을 descriptor(메타데이터)에 저장하는 방법: 하드웨어가 직접 제공하지 않고 소프트웨어적으로 구현되어 비효율적이지만, 융통성이 있다 ex. LISP, Prolog

<br>

- Enumerations: 값의 나열을 정수 값의 나열로 표현한다 ex. `enum StudentClass {Fresh, Sophomore, Junior, Senior};`
- Boolean: 참 거짓을 나타내는 타입이다
	- 최소 주소 지정 단위인 byte나 word로 표현한다
	- 1bit만 사용하고 나머지를 무시하거나, nonzero만 true로 설정하는 방법 등이 있다
- character: 문자 하나
	- character를 모아놓은 집합으로는 ASCII code, utf-8 등이 있는데, 이들은 순서가 있다(collating sequence) 
- pointer: 다른 data structure의 주소를 가진다
	- 구현: heap storage management(할당/해지 반복하니까)

<br>

#### Numeric data types
: 숫자 사용이 많아, scalar data type 중 숫자 관련 type들만 따로 묶은 것
- Integer: 부호비트와 크기로 구성된다
	- descriptor(메타데이터)를 함께 저장할 수도 있고, 저장하지 않을 수도 있다(효율적)
	- descriptor를 함께 저장하는 경우, 별도의 word에 descriptor를 저장하고 포인터로 Integer를 가리킬 수도 있고, 한 word에 descriptor와 integer를 저장할 수도 있다
- Subrange: 정수값으로 된 범위를 지정할 수 있다 ex. 1..10
	- 저장공간을 줄일 수 있고, 더 강한 type checking이 가능하다
- Floating-point real numbers
	- 부호비트, 지수, 가수로 표현한다
	- real number에 대한 equality test(==)는 부정확하므로, 사용하지 말아야 한다
	- cf. fixed-point real number: dollar와 cent 표시를 위해, 고정 소수점 위치를 가지는 정수로 표현한다
		- 이때 실수로 표현 시 round-off error(소수점 아래 특정 부분 누락) 등의 문제가 발생할 수 있다

### structured data type
: 다른 data object의 조합으로 이뤄진 타입
	- 구조의 설계 및 구현, 저장공간 관리 방식을 결정하는 것이 중요하다

<br>

- 설계 시 고려할 요소들
	- 조합을 구성하는 data object의 개수: 구성하는 object의 수가 고정적인지(ex. array) 유동적인지(ex. stack). 최대 개수는 몇 개인지.
	- 각 data object의 타입: 구성하는 object들이 모두 같은 타입인지(ex. array) 다를 수 있는지(ex. records)
	- 접근 방식: array처럼 첨자로 접근할지, record처럼 내가 만든 식별자(field name)로 접근할지, stack처럼 특정 부분만 접근할 수 있도록 할지(top)
	- 조합 형태: 선형적으로 나열되어 있는지(ex. character string), 다차원 배열인지(ex. vector of vector)
- 구현 시 고려할 요소들
	- 저장되는 위치
		- object 수가 고정적이거나, 유동적이지만 타입이 모두 동일한 경우 하나의 블록에 저장한다. 이때 operation은 'base address + offset'의 형태로 접근한다
		- object 수가 유동적이면서 타입이 다를 수 있는 경우 포인터로 연결된 여러 블록에 나누어 저장한다. 이때 operation은 처음 블록에서부터 포인터를 따라서 접근한다

- Vector: 같은 타입의, 고정된 개수의 component들이 선형적으로 나열되어 있는 data structure
	- 속성(개수, 타입, 첨자)
	- operation(첨자로 값 접근, 생성, 해제, 값 수정, 사칙연산. 값 추가 할당 및 삭제는 불가)
> 첨자로 값 접근 시 A[i] = a + (i -lb) * E = (a - lb * E) + (i * E)이다
> a는 vector의 시작 주소, E는 각 component의 크기, lb는 시작 인덱스이다(언어마다 시작 인덱스가 다를 수 있기 때문)
> 결국 시작주소에서부터 크기만큼 첨자 번 이동하는 것이다
> 이때 lb, E는 컴파일 시간에, a는 loading 시간에, I는 컴파일 시간 또는 수행 시에 값이 결정된다
> 즉, a - lb * E는 a가 결정될 때 한번만 계산하면 된다
- 2-dimensional array
	- 구현(A[1..2, 1..3])
		- row-major order: A[1,1], A[1,2], A[1,3], A[2,1], A[2,2], A[2,3]
		- column-major order: A[1,1], A[2,1], A[1,2], A[2,2], A[1,3], A[2,3]
> 첨자로 값 접근 시 order에 따라 차이가 있다
> row-major order: A[i,j] = a + (i - LBi) * (UBj - LBj + 1) * E + (j - LBj) * E = (a - LBi * S - LBj * E) + i * S + j * E = V0 + i*S + j*E
> S는 행의 길이이고, V0는 a가 결정될 때 한번만 계산하면 된다
> column-major order: A[i,j] = a + (j - LBj) * (UBi - LBi + 1) * E + (i - LBi) * E = (a - LBj * S - LBi * E) + j * S + i * E = V0 + j*S + i*E
> S는 열의 길이이고, V0는 a가 결정될 때 한번만 계산하면 된다
- Record: 다른 타입의, 고정된 개수의 component들로 구성된 data structure
	- 속성(개수, 개별 타입, 접근자 `.`)
	- storage representation
		- 한 블록에 순서대로 저장된다
> 첨자로 값 접근 시 R.I = a + size(R.1) + size(R.2) + ... + size(R.I-1) = a + KI
> 이때 오프셋 KI는 번역 시간에 계산된다
	- +variant record: 두 타입 중 하나를 선택할 수 있다
		- 할당은 더 크기가 큰 타입의 크기로 하고, 수행 중 타입 값에 따라 일부만 사용하거나 전체를 사용한다(이때 타입 값이 작을 때 존재하지 않는 뒷부분을 access하는 문제가 있다)
- List: 타입도 다르고 개수도 유동적인, data structure들의 나열
	- List를 이용해 stack, queue, tree, directed graph, property list 등을 구현할 수 있다	
- Character strings: character의 나열
	- 종류
		- 1) 고정 길이(pascal cobol 등)
		- 2) declared bound(c 등): length 변할 수 있으나, bound 초과 시 truncate
		- 3) unbounded: 수행 중 길이 가변
	- operation(concatenation, substring)
	- 구현
		- 고정 길이: character 타입 vector
		- declared bound: 최대길이와 현재길이를 가지는 descriptor 활용
		- unbounded: 고정 길이 object들의 linked list 혹은 character 타입의 연속적인 array
- Set: 중복 없는 값을 순서 없이 가지고 있는 data object
	- operation(membership, insertion and deletion of single value, union, intersection, difference of set)
	- storage representation
		- bit-string: 전체 집합의 크기가 작을 때 효율적이다(있으면 1, 없으면 0)
			- membership: bit가 1인지 체크
			- insertion/deletion: bit 1/0 set
			- union/intersection: bit-or/bit-and
			- difference(A-B): A and (complement of B)
		- hash code: 전체 집합의 크기가 클 때 사용한다
			- membership, insertion/deletion: 효율적
			- union, intersection, difference: 비효율적
> hashing은 효율적인 검색이 목표이다
> hash table의 크기가 전체 집합 크기의 2배 이상일 때 효율적인 hashing이 가능하다
- File: 데이터 입출력과, 임시 저장소 역할을 담당하는 data structure로, 다른 data structure에 비해 크기가 크다
	- storage representation
		- 보통 HDD에 저장된다. 접근이 느리기 때문에 프로그램 종료 후에도 남아있다
	- 종류
		- sequential file: 가변 길이이고 최대 길이가 없고 같은 타입의 component의 나열
			- 위에서 아래의 순서로 접근할 수 있다
			- 읽고 쓸 위치를 지정하는 file-position pointer가 있다
			- 구현: os에 종속적이다
		- direct-access file: array처럼 접근할 수 있으나, 저장이 HDD에 되며 구현과 접근 연산이 array와 다르다
		- indexed sequential file: sequential file과 direct-access file의 중간

## Declaration
:실행 시 필요한 data object의 정보를 번역기(컴파일러/인터프리터)에 알려주는 statement
- object 자체의 경우 이름, 타입, 수명, 원소의 수 및 원소의 자료형 등이 필요하다
	- 명시적으로 선언할 수도 있고, 선언하지 않을 수도 있다(암묵적 선언)
- 내가 선언한 operation의 경우 signature(특징), 즉 argument와 result type을 해당 subprogram이 호출되기 전에 번역기에 알려줘야 한다
	- 이를 위해 C/C++에서는 함수 원형을 적어준다

### 목적
- 타입과 속성을 바탕으로 해당 data object를 저장하는 최적의 방식을 결정하여, 저장공간도 적게 사용하면서 실행 시간을 줄일 수 있다
- 수명을 바탕으로 data object를 저장할 최적의 위치를 결정하여, 저장공간 관리를 최적화할 수 있다
	- 수명을 모른다면, 모든 data object를 마치 전역변수처럼 별도의 공간에 할당해야 한다
- *정적 형 검사를 허용한다*

> ### Type checking
> : 수행되는 operation이 알맞은 타입의 인자를 알맞은 수만큼 받았는지 확인하는 작업이다
> - dynamic type checking(run-time type checking): 실행 중 특정 연산 하기 직전 type checking
>   	- 선언이 필요 없고, 타입을 신경 쓰지 않아도 되어 융통성이 좋다
>	- 실행 중 타입 정보를 요구하기에 추가 공간이 필요하고, 소프트웨어적으로 구현되기에 실행시간이 길어져 효율이 낮다
>	- ex. arr[i+j]에서 i+j의 값이 수행 시에 계산되기에, 이때의 subscript range error는 run-time checking으로 잡아야 한다
> - static type checking(compile-time type checking): 번역 시간에 generic operation(여기저기 쓰이는. 사칙연산 등)을 명시된 타입 간의 연산으로 연산을 전환하여, run-time type checking이 필요하지 않다
> 	- 사칙연산은 여러 타입 간 수행될 수 있기에, 다형성(매개변수 타입, 리턴타입 다르면 다른 프로토타입)을 활용해 오버로딩되어 있다
> 	- 신뢰성과 효율이 좋고, 융통성이 낮다
>	- compile time에는 해당 component의 존재 여부를 알 수 없기에, 존재한다는 가정 하에 type이 옳다는 것만 guarantee할 수 있다
> <br>
> 
> ### Type conversion and coercion
> - explicit type conversion: built-in type conversion operator를 이용한다
> - coercion(implicit type conversion): 타입이 맞지 않는 경우, 암묵적으로 conversion이 일어난다
> 	- 이때 정보 손실이 있으면 안되기에, 비트수가 많은 쪽에서 적은 쪽으로 축소하는(narrowing. float -> int) 형변환은 허용되지 않고 widening(short int -> int)만 허용된다

## Assignment
: data object에 바인딩된 값을 바꾼다(머신 state를 변화시키는 부작용도 있다)

- ex. `X := X`
	- 주소를 의미하는 left-hand side value와, 값을 의미하는 right-hand side value
	- l-value를 계산. r-value를 계산. l-value에 r-value를 할당. (바인딩이 잘 되었다는 의미로) r-value를 리턴한다
