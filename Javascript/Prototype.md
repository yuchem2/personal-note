## 서론
객체 지향 언어는 크게 **클래스 기반**과 **프로토타입 기반**으로 나눌 수 있습니다.

**클래스 기반 언어(Java, C++, C# 등)**에서는 객체를 생성하기 전에 반드시 클래스를 정의하고, 객체는 그 클래스를 기반으로 생성됩니다. 상속 또한 클래스 간에 정의되어 정적인 구조를 가집니다.

반면, **프로토타입 기반 언어(JavaScript 등)**에서는 클래스 없이 객체가 다른 객체를 원형(prototype)으로 삼아 동적으로 상속할 수 있습니다. 이를 통해 객체는 다른 객체의 속성과 메서드를 재사용할 수 있으며, 이 동작의 핵심이 되는 메커니즘이 바로 `Prototype`입니다.
## 프로토타입 기반 언어?
프로토타입 기반 언어는 모든 객체들이 메소드와 속성들을 상속받기 위한 템플릿으로써 **프로토타입 객체**를 가집니다. 프로토타입 객체도 또 다시 상위 프로토타입 객체로부터 메소드와 속성을 상속받을 수도 있고 그 상위 프로토타입 객체도 마찬가지입니다. 이를 **프로토타입 체인(prototype chain)**이라고 하며 다른 객체에 정의된 메소드와 속성을 한 객체에서 사용할 수 있도록 하는 근간입니다. **즉, 상속되는 속성과 메소드들은 각 객체가 아니라 객체의 생성자의 `prototype` 이라는 속성에 정의되어 있습니다.**

Javascript에서는 객체 인스턴스와 프로토타입 간의 연결(많은 브라우저들의 생성자의 `prototype` 속성에서 파생된 `__proto__` 속성으로 객체 인스턴스에 구현하고 있습니다.)이 구성되며 이 연결을 따라 프로토타입 체인을 타고 올라가며 속성과 메소드를 탐색합니다.

## 프로토타입 객체
아래와 같은 `Person()` 생성자가 존재한다고 가정하고 다음과 같이 객체를 생성할 수 있습니다.

```jsx
function Person(first, last, age, gender, interests) {
	this.name = {
    'first': first,
    'last' : last
  };
  this.age = age;
  this.gender = gender;
  this.interests = interests;
  this.bio = function() {
    // ... 함수 구현
  };
  this.greeting = function() {
    alert('Hi! I\\'m ' + this.name.first + '.');
  };
};

var person1 = new Person("Bob", "Smith", 32, "male", ["music", "skiing"]);
```

그 후 `person1` 객체를 출력하게 되면, `person1` 의 프로토타입 객체인 `Person()` 에 정의된 멤버들 - `name`, `age`, `gender`, `interests`, `bio`, `greeting` 을 확인할 수 있습니다. 또한 `watch` , `valueOf` 와 같은 `Person()` 의 프로토타입 객체인 `Object` 에 정의된 다른 멤버들도 확인할 수 있습니다. 즉, 프로토타입 체인이 동작하고 있다는 증거입니다. `person1.valueOf()` 를 호출하면 프로토타입 체인을 통해 다음과 같은 일이 발생합니다.

1. 브라우저는 우선 `person1` 객체가 `valueOf()` 메소드를 가지고 있는지를 확인합니다.
2. `valueOf()` 메서드가 없음으로, `person1` 의 프로토타입 객체인 `Person()` 생성자의 프로토타입에 `valueOf()` 메소드가 있는지 확인합니다.
3. 여전히 없음으로, `Person()` 생성자의 프로타입 객체의 프로토타입 객체인 `Object()` 생성자의 프로토타입에 `valueOf()` 메소드가 있는지 확인합니다.

Javscript 언어 표준 스펙에서 `[[prototype]]` 으로 표현되는 프로토타입 객체에 대한 “링크”는 내부 속성으로 정의되어 있습니다. 그러나 특정 객체의 프로토타입에 직접 접근하는 공식적인 방법은 없습니다. 하지만 많은 브라우저에서 `__proto__` 속성을 통해 접근할 수 있도록 구현하였습니다. ECMAScript 2015 부터는 `Object.getPrototypeOf(obj)` 함수를 통해 객체의 프로토타입 객체에 바로 접근할 수 있습니다.

```jsx
person1.__proto__ // Object.getPrototypeOf(person1)
person1.__proto__.__proto__ // Object.getPrototypeOf(Object.getPrototypeOf(person1))
```

|구분|속성|소속|설명|
|---|---|---|---|
|개별 객체의 프로토타입|`Object.getPrototypeOf(obj)` or `obj.__proto__`|인스턴스(객체)|해당 객체가 상속받는 원형 객체를 가르킵니다.|
|생성자의 프로토타입|`constructor.prototype`|생성자 함수|생성자 함수로 생성된 객체들이 공유하는 프로토타입 객체입니다.|
### 프로토타입 속성
상속받은 속성과 메소드들은 `prototype` 속성(sub-namespace)에 저장되어 있습니다. `prototype` 속성 또한 하나의 객체이며, 프로토타입 체인을 통해 상속하고자 하는 속성과 메소드를 담아두는 버킷으로 주로 사용되는 객체입니다. 상속될 속성과 메소드들은 다음과 같이 정의할 수 있습니다.

```jsx
Person.prototype.farewell = function() {
	alert(this.name.first + " has left the building. Bye for now!");
}
```

`prototype` 에 새 메소드를 추가하는 순간 동일한 생성자로 생성된 모든 객체에서 추가된 메서드를 바로 사용할 수 있습니다. 즉, 속성과 메서드를 정의하고, 객체를 생성한 후에도 `prototype` 에 새 메서드, 속성을 추가하더라도 그 객체에서 바로 그 속성과 메서드를 사용할 수 있습니다. (원문에는 자동으로 서술되어 있다고 기록되어 있지만, 실제로는 프토로타입 속성을 모든 인스턴스에서 공유하기 때문에 정의하는 즉시 별도의 갱신 과정 없이 접근이 가능합니다.)

`prototype` 속성에는 메서드 말고도 속성도 다음과 같은 방법으로 추가할 수 있습니다.

```jsx
Person.prototype.fullName = "Bob Simith"; // 모든 person의 이름이 Bob Simith가 되므로 좋은 방법이 아닙니다.
Person.prototype.fullName = this.name.first + " " + this.name.last; // `this`는 Person 인스턴스가 아닌 전역 객체를 가리키게 되어 "undefined undefined"라는 결과만 확인할 수 있습니다
```

프로토타입에 상수를 정의하는 것은 가능하지만, 일반적으로 생성자에서 정의하는 것이 낫습니다. 일반적인 방식으로는 속성은 생성자에서, 메서드는 프로토타입에서 정의합니다. 생성자에는 속성에 대한 정의만 있으며 메소드는 별도의 블럭으로 구분할 수 있어 코드 가독성이 높아집니다.

### 생성자 속성
모든 생성자 함수는 `constructor` 속성을 지닌 객체를 프로토타입 객체로 가지고 있습니다. `constructor` 속성은 원본 생성자 함수 자신을 가리키고 있습니다. 즉, `person1.constructor()` 과 `new Person()` 은 동일하게 작동합니다. 이렇게 생성자를 호출하는 방식은 자주 활용하지 않지만, 실행 중 명시적인 생성자 함수를 예측할 수 없는 상황에서 인스턴스를 생성해야 하는 경우 유용하게 활용할 수 있습니다.

### 프로토타입 상속
특정 객체를 상속받기 위해서는 JS에서는 `call` 메서드를 이용해 다음과 같이 정의합니다.

```jsx
function Brick() {
	this.width = 10;
	this.height = 10;
}

function BlueGlassBrick() {
	Brick.call(this);
	this.opacitiy = 0.5;
	this.color = "blue";
}
```

하지만, 이와 같이 생성한 경우 `BlueGlassBrick` 생성자에는 생성자 함수 자신에 대한 참조만 가지고 있는 프로토타입 속성이 할당되어 있습니다. 정작 상속받은 `Brick` 생성자의 `prototype` 속성은 없습니다. `Brick` 의 메서드도 상속받기 위해서는 다음과 같은 코드를 추가해야 합니다.

```jsx
BlueGlassBrick.prototype = Object.create(Brick.prototype); 
BlueGlassBrick.prototype.constructor = BlueGlassBrick; // 위 코드만 작성한 경우 BlueGlassBrick.prototype의 constructor 속성은 Brick()으로 되어 있습니다. 이는 문제의 소지가 있음으로 생성자를 변경해줍니다.
```

### ECMAScript 2015 Class
ECMAScript 2015 이후에는 클래스 문법을 공개하여 클래스를 조금 더 쉽게 작성할 수 있게 되었습니다. Class-스타일로 `Person` 을 다음과 같이 정의할 수 있습니다.

```jsx
class Person {
	constructor(first, last, age, gender, interests) { // 생성자 함수
    this.name = {
      first,
      last
    };
    this.age = age;
    this.gender = gender;
    this.interests = interests;
  }

  greeting() { // 메소드
    console.log(`Hi! I'm ${this.name.first}`);
  }

  farewell() { // 메소드
    console.log(`${this.name.first} has left the building. Bye for now!`);
  }
}

class Teacher extends Person {
	constructor(first, last, age, gender, interests, subject, grade) {
    super(first, last, age, gender, interests); // Person() 생성자 호출

    // subject and grade are specific to Teacher
    this.subject = subject;
    this.grade = grade;
  }
}
```

위오 같이 정의한 경우 `prototype` 체인과 `constructor` 설정을 모두 자동으로 처리하게 되어 앞서 생성자 함수를 사용한 방법처럼 수동처리를 할 필요가 없게 됩니다.

## 참고자료
- [https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes)