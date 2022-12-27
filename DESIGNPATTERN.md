# 5principles of oop

## SRP(Single-responsibility principle)

### 개념
>단일 책임 원칙은 클래스, 모듈, 함수 등 모든 하나의 책임, 목적을 하는 것이다.  
>every class should have only one reason to change


<br></br>
## Iterator Pattern(반복자 패턴)
### 개념 
보통 모든 배열 또는 컨테이너를 원소 0번에서 n-1번까지 반복하기 위해 **for(int i = 0; i < n; i++)** 로 반복한다. 반복자 패턴은 반복문에서 변수 i의 기능을 추상화한 패턴이다. 

### 반복자 패턴은 장점은?
단순히 추상화해서 내부 구현을 외부로 누출되지 않다는 것은 반복자 패턴 뿐만 아니라 모든 패턴이 지향해야하는 관점이다. 내가 사용한 반복자 패턴의 강점은 컨테이너의 원소를 삭제할 경우이다. 단순한 반복문으로 해당 컨테이너의 원소를 삭제하게되면 제대로된 삭제가 되지 못하거나 에러를 발생시킬 수 있다. 내부적으로 컨테이너의 모양이 변경되는 경우가 다수 있기 때문이다. 그렇기에 단순 반복문으로 삭제하면 많은 에로사항이 따른다. 하지만 Iterator를 이용하여 삭제할 경우 코드가 매우 단순하고 에러를 다루를 코드를 따로 생각할 필요가 없다. 반복자의 함수는 매우 명확하게 작성되어 그 기능을 최대한 활용하면 코드의 간결성이 높아진다. 또한 반복하는 방법이 단순 순회가 아니라 전위 순회, 중위 순회처럼 다른 방법으로 동작해야한다면 사용자가 구현할 필요없이 반복자의 반복 알고리즘을 작성하여 사용자가 반복하는 방법을 구현하지 않아도 된다.

### 구조
<p align="center"><img width="496" alt="image" src="https://user-images.githubusercontent.com/56042451/209600598-80f43972-cb8f-4a85-92db-46522c09d3fb.png"></p> 

* Aggregate : 반복자를 생성해준다. 왜 인터페이스냐? vector같은 컨테이너에 반복자를 생성해주는 것을 강제화 시키고 틀을 잡아놓으면 개편하겠지? 왜냐? 컨테이너는 벡터 말고도 여러개 만들어 질게 분명하거든!
* BookShelf(ConcreteAgreegate) : Aggregate를 구체화시키는 클래스로 흔히 vector, queue같은 컨테이너이다.
* Iterator : 반복자에 대한 틀이다. 왜 인터페이스냐? 반복자가 가지는 변수가 concreteAggregate마다 다르기 그냥 인터페이스로 가져야할 메소드를 강제화 시키는 것이 좋다.
* BookShelfIterator(ConcreteIterator) : Iterator를 BookShelf에 맞게 구체화 시킨 클래스이다. 벡터면 벡터반복자, 큐면 큐반복자. 구체화된 반복자와 구체화된 집약은 나름 강하게 연결되어 있지만 인터페이스 간에는 남남이다.



