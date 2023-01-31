---
description: '- 디자인 패턴 자료 정리'
---

# Index



* Javascript Design Patterns&#x20;
  * [https://www.freecodecamp.org/news/javascript-design-patterns-explained/](https://www.freecodecamp.org/news/javascript-design-patterns-explained/)
* 디자인 패턴&#x20;
  * 객체 지향 프로그래밍의 기능과 함정을 탐구하고 일반적인 프로그래밍 문제를 해결하기 위해 구현할 수 있는 23가지의  유용한 패턴을 설명하는 책에서 나옴&#x20;
*   알고리즘이나 특정 구현이 아님&#x20;

    * 특정 종류의 문제를 해결하기 위해 특정 상황에서 유용할 수 있는 아이디어, 의견 및 추상화에 가까움&#x20;
    * 패턴의 특정 구현은 다양한 요인에 따라 달라질 수 있음&#x20;
      * 그 배후에 있는 개념과 문제에 대한 더 나은 솔루션을 달성하는 데 어떻게 도움이 될 수 있는지


* OOP C++ 프로그래밍을 염두에 두고 생각해낸 것&#x20;
  * Javascript와 같은 언어에서는 이러한 패턴은 똑같이 유용하지 않을 수 있음&#x20;
  * 코드에 불필요한 상용구를 추가할 수도 있음&#x20;
* 생성, 구조, 행동 패턴의 세 가지 주요 범주로 분류됨&#x20;

### 생성 디자인 패턴&#x20;

* 생성 패턴은 객체를 생성하는 데 사용되는 다양한 메커니즘으로 구성됨&#x20;

#### 싱글톤 패턴&#x20;

* 클래스가 변경 불가능한 인스턴스를 하나만 갖도록하는 디자인 패턴&#x20;
* 간단히 말해서 싱글톤 패턴은 복사하거나 수정할 수 없는 객체로 구성됨&#x20;
* 어플리케이션의 불변의 단일 지점을 원할 때 종종 유용&#x20;
*   ex) App의 모든 구성을 단일 객체에 포함하고 싶다고 가정&#x20;

    * 그 객체의 복제나 수정을 허용하지 않기를 원한다면?
    * 이 패턴을 구현하는 2가지 방법 => 객체 리터럴과 클래스를 사용하는 것&#x20;





* 객체 리터럴을 이용하는 예

```javascript
const Config = {
    start: () => console.log('App has started'),
    update: () => console.log('App has updated')
}

Object.freeze(Config)


Config.start()
Config.update()

Config.name = "Robert"
console.log(Config)
```



* 클래스를 이용하는 예

```javascript
class Config {
    
    constructor() {}
    start(){
        console.log('App has started')
    }
    update(){
        console.log('App has updated')
    }

}

const instance = new Config()
Object.freeze(instance)


```



#### 팩토리 메서드 패턴&#x20;

* 객체 생성 후 수정할 수 있는 객체 생성을 위한 인터페이스를 제공&#x20;
* 객체를 생성하는 논리가 한 곳에서 중앙 집중화되어 코드를 단순화하고 더 잘 구성한다는 것&#x20;
* 많이 사용되며 클래스 또는 팩토리 함수(객체를 반환하는 함수)를 통해 두 가지 다른 방식으로 구현할 수도 있음&#x20;

```javascript
class Alien {
    constructor(name, phrase) {
        this.name = name
        this.phrase = phrase
        this.species = "alien"
    }
    fly = () => console.log("Zzzzzziiiiiinnnnnggggg!!")
    sayPhrase = () => console.log(this.phrase)
}

const alien1 = new Alien("Ali", "I'm Ali the alien!")
console.log(alien1.name) 
```



* 팩토리 함수를 이용하는 예

```javascript
function Alien(name, phrase) {
    this.name = name
    this.phrase = phrase
    this.species = "alien"

}

Alien.prototype.fly = () => console.log("Zzzzzziiiiiinnnnnggggg!!")
Alien.prototype.sayPhrase = () => console.log(this.phrase)

const alien1 = new Alien("Ali", "I'm Ali the alien!")

console.log(alien1.name)
console.log(alien1.phrase)
alien1.fly()


```



#### 추상 팩토리 패턴&#x20;

* 이 패턴을 사용하면 구체적인 클래스를 지정하지 않고&#x20;
  * 관련 객체의 패밀리를 생성할 수 있음&#x20;
  * 일부 속성과 메서드만 공유하는 객체를 만들어야 하는 상황에서 유용&#x20;
* 작동 방식 - 클라이언트가 상호작용하는 추상 팩토리를 제시하는 것&#x20;
* 해당 추상 팩토리는 해당 논리가 지정된 해당 구체적인 팩토리를 호출
  * 그 구체적인 팩토리는 최종 객체를 반환하는 팩토리&#x20;
* 기본적으로, 팩토르 메서드 패턴 위에 추상화 계층을 추가해 다양한 유형의 객체를 생성할 수 있지만&#x20;
  * 여전히 단일 팩토리 함수 또는 클래스와 상호 작용할 수 있음&#x20;

```javascript
// We have a class or "concrete factory" for each vehicle type
class Car {
    constructor () {
        this.name = "Car"
        this.wheels = 4
    }
    turnOn = () => console.log("Chacabúm!!")
}

class Truck {
    constructor () {
        this.name = "Truck"
        this.wheels = 8
    }
    turnOn = () => console.log("RRRRRRRRUUUUUUUUUMMMMMMMMMM!!")
}

class Motorcycle {
    constructor () {
        this.name = "Motorcycle"
        this.wheels = 2
    }
    turnOn = () => console.log("sssssssssssssssssssssssssssssshhhhhhhhhhham!!")
}

// And and abstract factory that works as a single point of interaction for our clients
// Given the type parameter it receives, it will call the corresponding concrete factory
const vehicleFactory = {
    createVehicle: function (type) {
        switch (type) {
            case "car":
                return new Car()
            case "truck":
                return new Truck()
            case "motorcycle":
                return new Motorcycle()
            default:
                return null
        }
    }
}

const car = vehicleFactory.createVehicle("car") // Car { turnOn: [Function: turnOn], name: 'Car', wheels: 4 }
const truck = vehicleFactory.createVehicle("truck") // Truck { turnOn: [Function: turnOn], name: 'Truck', wheels: 8 }
const motorcycle = vehicleFactory.createVehicle("motorcycle") // Motorcycle { turnOn: [Function: turnOn], name: 'Motorcycle', wheels: 2 }
```



#### 빌더 패턴&#x20;

* 단계에서 객체를 만드는데 사용되는 패턴&#x20;
* 일반적으로 객체에 특정 속성이나 메서드를 추가하는 함수나 메서드가 있음&#x20;
* 속성과 메서드 생성을 다른 엔터티로 분리한 것&#x20;
* 클래스 or 팩토리 함수가 있는 경우 인스턴스화하는 객체에는 항상 해당 클래스/팩토리에서 선언된 모든 속성과 메서드가 존재&#x20;
* But, 빌더 패턴을 사용하면 객체를 만들고 필요한 `단계` 만 적용할 수 있음 => 보다 유연한 접근 방식&#x20;
* Object Composition과 매우 관련있음&#x20;

```javascript
// We declare our objects
const bug1 = {
    name: "Buggy McFly",
    phrase: "Your debugger doesn't work with me!"
}

const bug2 = {
    name: "Martiniano Buggland",
    phrase: "Can't touch this! Na na na na..."
}

// These functions take an object as parameter and add a method to them
const addFlyingAbility = obj => {
    obj.fly = () => console.log(`Now ${obj.name} can fly!`)
}

const addSpeechAbility = obj => {
    obj.saySmthg = () => console.log(`${obj.name} walks the walk and talks the talk!`)
}

// Finally we call the builder functions passing the objects as parameters
addFlyingAbility(bug1)
bug1.fly() // output: "Now Buggy McFly can fly!"

addSpeechAbility(bug2)
bug2.saySmthg() // output: "Martiniano Buggland walks the walk and talks the talk!"
```



#### 프로토타입 패턴&#x20;

* 다른 객체를 청사진으로 사용해 객체의 속성과 메서드를 상속하는 객체를 만들 수 있음&#x20;
* 최종 결과는 클래스를 사용하여 얻는 것과 매우 유사하지만 동일한 클래스에 의존하지 않고&#x20;
  * 객체 간에 속성과 메서드를 공유할 수 있기 때문에 약간 더 유연

```javascript
// We declare our prototype object with two methods
const enemy = {
    attack: () => console.log("Pim Pam Pum!"),
    flyAway: () => console.log("Flyyyy like an eagle!")
}

// We declare another object that will inherit from our prototype
const bug1 = {
    name: "Buggy McFly",
    phrase: "Your debugger doesn't work with me!"
}

// With setPrototypeOf we set the prototype of our object
Object.setPrototypeOf(bug1, enemy)

// With getPrototypeOf we read the prototype and confirm the previous has worked
console.log(Object.getPrototypeOf(bug1)) // { attack: [Function: attack], flyAway: [Function: flyAway] }

console.log(bug1.phrase) // Your debugger doesn't work with me!
console.log(bug1.attack()) // Pim Pam Pum!
console.log(bug1.flyAway()) // Flyyyy like an eagle!
```



### 구조 디자인 패턴&#x20;

* 객체와 클래스를 더 큰 구조로 조합하는 방법&#x20;

#### 어댑터 패턴&#x20;

* 어댑터를 사용하면 호환되지 않는 인터페이스가 있는 두 객체가 서로 상호 작용할 수 있음&#x20;
* ex) 애플리케이션이 XML을 반환하는 API를 참조하고 해당 정보를 처리하기 위해 해당 정보를 다른 API로 전송한다고 가정
  * 그러나 처리 API는 JSON을 예상&#x20;
  * 두 인터페이스가 호환되지 않기 때문에 받은 정보를 그대로 보낼 수는 없음&#x20;
  * 먼저 adaption이 필요&#x20;
* 더 간단한 예를 통해 동일한 개념을 시각화할 수 있음&#x20;
  * ex) 도시 배열과 해당 도시 중 가장 많은 거주민 수를 반환하는 함수가 있다고 가정&#x20;
    * 배열에 있는 거주민의 수는 수백만 명이지만, 백만 명의 전환 없이 거주민이 있는 새로운 도시를 추가해야 함&#x20;

```javascript
// Our array of cities
const citiesHabitantsInMillions = [
    { city: "London", habitants: 8.9 },
    { city: "Rome", habitants: 2.8 },
    { city: "New york", habitants: 8.8 },
    { city: "Paris", habitants: 2.1 },
] 

// The new city we want to add
const BuenosAires = {
    city: "Buenos Aires",
    habitants: 3100000
}

// Our adapter function takes our city and converts the habitants property to the same format all the other cities have
const toMillionsAdapter = city => { city.habitants = parseFloat((city.habitants/1000000).toFixed(1)) }

toMillionsAdapter(BuenosAires)

// We add the new city to the array
citiesHabitantsInMillions.push(BuenosAires)

// And this function returns the largest habitants number
const MostHabitantsInMillions = () => {
    return Math.max(...citiesHabitantsInMillions.map(city => city.habitants))
}

console.log(MostHabitantsInMillions()) // 8.9
```

#### &#x20;데코레이터 패턴&#x20;

* behavior가 포함된 래퍼 객체 내부에 새 동작을 배치하여 객체에 새 동작을 연결할 수 있음&#x20;
* React 및 고차 구성 요소(HOC)에 어느 정도 익숙하다면 적합할 것&#x20;
* 기술적으로, React의 Component는 객체가 아니라 함수입니다&#x20;
* React Context 또는 Memo에 대해 생각해보면 Component를 이 HOC에 자식으로 전달하고 있으며&#x20;
  * 덕분에 이 자식 Component가 특정 기능에 액세스할 수 있음을 알 수 있음&#x20;
* ex) ContextProvider가 props로 자식을 받는 예

```javascript
import { useState } from 'react'
import Context from './Context'

const ContextProvider: React.FC = ({children}) => {

    const [darkModeOn, setDarkModeOn] = useState(true)
    const [englishLanguage, setEnglishLanguage] = useState(true)

    return (
        <Context.Provider value={{
            darkModeOn,
            setDarkModeOn,
            englishLanguage,
            setEnglishLanguage
        }} >
            {children}
        </Context.Provider>
    )
}

export default ContextProvider
```

* 그런 다음 전체 애플리케이션을 둘러쌈

```javascript
export default function App() {
  return (
    <ContextProvider>
      <Router>

        <ErrorBoundary>
          <Suspense fallback={<></>}>
            <Header />
          </Suspense>

          <Routes>
              <Route path='/' element={<Suspense fallback={<></>}><AboutPage /></Suspense>}/>

              <Route path='/projects' element={<Suspense fallback={<></>}><ProjectsPage /></Suspense>}/>

              <Route path='/projects/helpr' element={<Suspense fallback={<></>}><HelprProject /></Suspense>}/>

              <Route path='/projects/myWebsite' element={<Suspense fallback={<></>}><MyWebsiteProject /></Suspense>}/>

              <Route path='/projects/mixr' element={<Suspense fallback={<></>}><MixrProject /></Suspense>}/>

              <Route path='/projects/shortr' element={<Suspense fallback={<></>}><ShortrProject /></Suspense>}/>

              <Route path='/curriculum' element={<Suspense fallback={<></>}><CurriculumPage /></Suspense>}/>

              <Route path='/blog' element={<Suspense fallback={<></>}><BlogPage /></Suspense>}/>

              <Route path='/contact' element={<Suspense fallback={<></>}><ContactPage /></Suspense>}/>
          </Routes>
        </ErrorBoundary>

      </Router>
    </ContextProvider>
  )
}
```



* 나중에 `useContext` hook을 사용해 애플리케이션의 모든 Component에서 Context에 정의된 상태에 액세스할 수 있음&#x20;

```javascript
const AboutPage: React.FC = () => {

    const { darkModeOn, englishLanguage } = useContext(Context)
    
    return (...)
}

export default AboutPage
```

* 특정 기능에 액세스할 수 있도록 객체를 다른 객체 안에 배치함&#x20;

#### 파사드 패턴&#x20;

*



