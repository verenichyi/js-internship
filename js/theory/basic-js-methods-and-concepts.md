# Основные методы и концепции Javascript

### Введение:

JavaScript — это высокоуровневый язык программирования, проделавший путь от простого языка для часиков на страницах сайтов
до мощного инструмента, на котором разрабатывается практически всё: клиентская и серверная части приложений, мобильные и
компьютерные программы и даже игры. Тем не менее, несмотря на всю масштабность языка, в реальной разработке чистый JavaScript
практически не используется. Для помощи и ускорения работы программистов существует множество библиотек и фреймворков.

В веб-разработке, в частности, ее клиентской составляющей, самой популярной библиотекой является React. На ней написаны,
без преувеличения, миллионы сайтов и приложений, на ней же будете писать и вы. Все части React написаны на JavaScript, в
том числе и HTML, именуемый JSX и являющийся синтаксическим расширением JavaScript. Поэтому, прежде чем приступать к изучению
React, необходимо понять основные методы и концепции самого JavaScript. Этим мы сейчас и займемся.

### Содержание:

- [*Функции обратного вызова (Callback)*](#функции-обратного-вызова-callback)
- [*Промисы (Promises)*](#промисы-promises)
- [*Метод массива Map()*](#метод-массива-map)
- [*Методы массива Filter() и Find()*](#методы-массива-filter-и-find)
  - [Array.filter()](#arrayfilter)
  - [Array.find()](#arrayfind)
- [*Деструктурирующее присваивание массивов и объектов*](#деструктурирующее-присваивание-массивов-и-объектов)
- [*Остаточные параметры и оператор расширения (Rest и Spread)*](#остаточные-параметры-и-оператор-расширения-rest-и-spread))
  - [Rest](#rest)
  - [Spread](#spread)
- [*Конструктор Set()*](#конструктор-set)
- [*Динамические ключи объектов*](#динамические-ключи-объектов)
- [*Метод массива Reduce()*](#метод-массива-reduce)
- [*Оператор опциональной последовательности ?.*](#оператор-опциональной-последовательности-)
- [*Fetch API*](#fetch-api)
- [*Async/Await*](#asyncawait)
---

### Функции обратного вызова (Callback)

Функция обратного вызова (Callback) — это функция, переданная в другую функцию в качестве аргумента, которая затем вызывается 
по завершении какого-либо действия.

Такие функции активно используются в методах массива (map(), filter(), и т. д.), setTimeout(), прослушивателях событий 
(click, scroll и т. д.) и многих других местах.

Например:

```javascript
function greeting(name) {
  alert('Hello ' + name);
}

function processuserInput(callback) {
  const name = prompt('Please enter your name.');
  callback(name);
}

processuserInput(greeting);
```
Подробнее о callback-функциях можно прочитать [здесь](https://developer.mozilla.org/ru/docs/Glossary/Callback_function).

### Промисы (Promises)

Давайте попытаемся отобразить в консоли 5 имен через 2 секунды каждое – то есть первое имя появится через 2 секунды, 
второе - через 4 секунды и так далее... :

```javascript
setTimeout(() => {
    console.log("Joel");
    setTimeout(() => {
        console.log("Victoria");
        setTimeout(() => {
            console.log("John");
            setTimeout(() => {
                console.log("Doe");
                setTimeout(() => {
                    console.log("Sarah");
                }, 2000);
            }, 2000);
        }, 2000);
    }, 2000);
}, 2000);
```
Приведенный выше пример будет работать, но его будет трудно понять, отладить или даже добавить обработку ошибок. Это 
называется __"Ад обратных вызовов"__. Ад обратных вызовов — это большая проблема, вызванная кодированием со сложными 
вложенными функциями обратного вызова.

Для предотвращения Ада обратных вызовов используются промисы. Промис – это объект, возвращающий значение, которое вы 
ожидаете увидеть в будущем, но не видите сейчас. Благодаря промисам можно писать асинхронный код синхронным образом.

Возможно, вы ломаете голову, задаваясь вопросом, в чем разница между синхронным и асинхронным. Проще говоря, 
синхронность означает, что задачи выполняются одна за другой. Асинхронность означает, что задачи выполняются независимо.

Синтаксис промиса JavaScript:

```javascript
const myPromise = new Promise((resolve, reject) => {  
    // ваш код
});
```
Промисы  имеют два параметра: один для успеха (resolve) и один для неудачи (reject). У каждого есть условие, которое должно
быть выполнено для того, чтобы промис был выполнен – в противном случае он будет отклонен:

```javascript
const myPromise = new Promise((resolve, reject) => {  
    const condition = 5;
    
    if(condition > 5) {    
        resolve('Promise is resolved successfully.');  
    } else {    
        reject(new Error('Promise is rejected'));  
    }
});
```
Существует 3 состояния объекта Promise:
- __Pending__ – по умолчанию это начальное состояние, предшествующее успешному или неудачному выполнению промиса
- __Resolved__ – операция завершена успешно
- __Rejected__ – операция завершена с ошибкой

Наконец, давайте попробуем повторно реализовать Ад обратных вызовов как промис:

```javascript
function addName (time, name){
  return new Promise ((resolve, reject) => {
    if(name){
      setTimeout(()=>{
        console.log(name)
        resolve();
      },time)
    }else{
      reject('No such name');
    }
  })
}

addName(2000, 'Joel')
  .then(()=>addName(2000, 'Victoria'))
  .then(()=>addName(2000, 'John'))
  .then(()=>addName(2000, 'Doe'))
  .then(()=>addName(2000, 'Sarah'))
  .catch((err)=>console.log(err));
```
Подробнее о Promise можно прочитать [здесь](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Promise).

### Метод массива Map()

Одним из наиболее часто используемых методов является map(), который позволяет выполнять итерации по массиву и изменять
его элементы с помощью функции обратного вызова. Функция обратного вызова будет выполняться для каждого элемента массива.

Предположим, у нас есть массив пользователей, содержащий их информацию:

```javascript
const users = [
  { firstName: "Susan", lastName: "Steward", age: 14, hobby: "Singing" },
  { firstName: "Daniel", lastName: "Longbottom", age: 16, hobby: "Football" },
  { firstName: "Jacob", lastName: "Black", age: 15, hobby: "Singing" }
];
```
Мы можем выполнить цикл с помощью map() и изменить его выходные данные:

```javascript
const reworkedusers = users.map((user)=>{
  // Объединим firstName и lastName
  const fullName = user.firstName + ' ' + user.lastName;
  
  return `
    <h3 class='name'>${fullName}</h3>
    <p class="age">${user.age}</p>
  `
});
```
> Важно отметить, что:
> - map() всегда возвращает новый массив, даже если это пустой массив
> - map() не изменяет размер исходного массива по сравнению с методом filter()
> - map() всегда использует значения из вашего исходного массива при создании нового
> - map() работает почти так же, как и любой другой итератор JavaScript, такой как forEach(), но правильно всегда использовать 
метод map() всякий раз, когда вы собираетесь возвращать значение

Одна из ключевых причин, по которой мы используем map(), заключается в том, что мы можем инкапсулировать наши данные в 
HTML, тогда как для React это просто делается с помощью JSX.

Подробнее об Array.map() можно прочитать [здесь](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/map).

### Методы массива Filter() и Find()
#### Array.filter()

Filter() предоставляет новый массив в зависимости от определенных критериев. В отличие от map(), он может изменять размер
нового массива, тогда как find() возвращает только один экземпляр (это может быть объект или элемент). Если существует 
несколько совпадений, он возвращает первое совпадение – в противном случае он возвращает undefined.

Предположим, у вас есть массивная коллекция зарегистрированных пользователей разного возраста:
```javascript
const users = [
  { firstName: "Susan", age: 14 },
  { firstName: "Daniel", age: 16 },
  { firstName: "Bruno", age: 56 },
  { firstName: "Jacob", age: 15 },
  { firstName: "Sam", age: 64 },
  { firstName: "Dave", age: 56 },
  { firstName: "Neils", age: 65 }
];
```
Вы можете отсортировать эти данные по возрастным группам, таким как молодежь (в возрасте от 1 до 15 лет), пожилые люди 
(в возрасте 50–70 лет) и так далее...

В этом случае метод filter() пригодится, поскольку он создает новый массив на основе критериев. Давайте посмотрим, как это 
работает:
```javascript
// молодые
const youngPeople = users.filter((person) => person.age <= 15);

//пожилые
const seniorPeople = users.filter((person) => person.age >= 50);
```
Filter() создает новый массив с подходящими значениями. Если условие не выполняется (нет совпадения), он создает пустой массив.

Подробнее об Array.filter() можно прочитать [здесь](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/filter).

#### Array.find()

Find(), как и filter(), выполняет итерацию по массиву в поисках экземпляра/элемента, который удовлетворяет указанному
условию. Как только он находит его, возвращает этот конкретный элемент массива и немедленно завершает цикл. Если совпадение 
не обнаружено, метод возвращает значение undefined. Например:

```javascript
const currentuser = users.find((person) => person.firstName === "Bruno");
```
Подробнее об Array.find() можно прочитать [здесь](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/find).

### Деструктурирующее присваивание массивов и объектов

Деструктурирующее присваивание, представленное в ES6, обеспечивает более быстрый и простой доступ и распаковку переменных 
из массивов и объектов.

До введения деструктуризации, если бы у нас был массив фруктов и мы хотели получить первый, второй и третий фрукты отдельно, 
нам бы пришлось делать что-то вроде этого:

```javascript
const fruits= ["Mango", "Pineapple", "Orange", "Lemon", "Apple"];

const fruit1 = fruits[0];
const fruit2 = fruits[1];
const fruit3 = fruits[2];

console.log(fruit1, fruit2, fruit3); //"Mango" "Pineapple" "Orange"
```
Это все равно что повторять одно и то же снова и снова, что достаточно неудобно. Давайте рассмотрим, как этот код можно 
изменить с использованием деструктуризации:

```javascript
const [fruit1, fruit2, fruit3] = fruits;

console.log(fruit1, fruit2, fruit3); //"Mango" "Pineapple" "Orange"
```
Если вы просто хотите получить первый и последний фрукты или второй и четвертый, нужно использовать запятые следующим образом:

```javascript
const [fruit1 ,,,, fruit5] = fruits;
const [,fruit2 ,, fruit4,] = fruits;
```
Давайте теперь посмотрим, как мы могли бы деструктурировать объект – потому что в React вы будете выполнять много операций 
по деструктуризации объектов.

Предположим, у нас есть объект user, который содержит имя, фамилию и прочее:

```javascript
const user = {
  firstName: "Susan",
  lastName: "Steward",
  age: 14,
  hobbies: {
    hobby1: "singing",
    hobby2: "dancing"
  }
};
```
По-старому получение этих данных могло быть напряженным и полным повторений:

```javascript
const firstName = user.firstName;
const age = user.age;
const hobby1 = user.hobbies.hobby1;

console.log(firstName, age, hobby1); //"Susan" 14 "singing"
```
Но с деструктуризацией всё намного проще:

```javascript
const {firstName, age, hobbies:{hobby1}} = user;

console.log(firstName, age, hobby1); //"Susan" 14 "singing"
```
Мы также можем сделать это внутри функции:

```javascript
function individualData({firstName, age, hobbies:{hobby1}}){
  console.log(firstName, age, hobby1); //"Susan" 14 "singing"
}

individualData(user);
```
Подробнее о деструктурирующем присваивании можно прочитать [здесь](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment).

### Остаточные параметры и оператор расширения (Rest и Spread)
#### Rest

Операторы JavaScript Rest и Spread используют три точки __...__ . Оператор Rest собирает элементы – он помещает оставшиеся 
определенные пользовательские значения в массив/объект JavaScript.

Предположим, у вас есть множество фруктов:

```javascript
const fruits = ["Mango", "Pineapple", "Orange", "Lemon", "Apple"];
```
Мы могли бы выполнить деструктуризацию, чтобы получить первый и второй фрукты, а затем поместить остальные фрукты в массив, 
используя оператор Кest:

```javascript
const [firstFruit, secondFruit, ...rest] = fruits;

console.log(firstFruit, secondFruit, rest); //"Mango" "Pineapple" ["Orange","Lemon","Apple"]
```
Посмотрев на результат, вы увидите первые два элемента изначального массива, третий же элемент — это массив, состоящий из 
оставшихся фруктов, которые мы не деструктурировали. Теперь мы можем выполнять любые операции с вновь сгенерированным 
массивом, например:

```javascript
const chosenFruit = rest.find((fruit) => fruit === "Apple");

console.log(`This is an ${chosenFruit}`); //"This is an Apple"
```
Важно иметь в виду, что Rest всегда должен быть последним (размещение очень важно).

С объектами Rest работает таким же образом:

```javascript
const currentuser = {
  firstName: "Susan",
  lastName: "Steward",
  age: 14,
  hobbies: {
    hobby1: "singing",
    hobby2: "dancing"
  }
};

const {age, ...rest} = currentuser;

console.log(age, rest);
```
Консоль выдаст следующий результат:

```shell
14
{
firstName: "Susan" ,
lastName: "Steward" ,
hobbies: {...}
}

```
#### Spread

Оператор расширения Spread используется для распределения элементов массива. Это дает нам возможность получить список 
параметров из массива. Оператор Spread имеет синтаксис, аналогичный оператору Rest, за исключением того, что он работает 
в противоположном направлении.

Оператор расширения эффективен только при использовании в литералах массива, вызовах функций или инициализированных объектах
свойств.

Например, предположим, что у вас есть массивы различных типов животных:

```javascript
const pets = ["cat", "dog", "rabbits"];

const carnivorous = ["lion", "wolf", "leopard", "tiger"];
```
Попробуем объединить эти два массива в один массив animal:

```javascript
const animals = [pets, carnivorous];

console.log(animals); //[["cat", "dog", "rabbits"], ["lion", "wolf", "leopard", "tiger"]]
```
Это не то, чего мы хотим – мы хотим, чтобы все элементы были только в одном массиве. И мы можем добиться этого, используя 
оператор расширения:

```javascript
const animals = [...pets, ...carnivorous];

console.log(animals); //["cat", "dog" , "rabbits", "lion", "wolf", "leopard", "tiger"]
```
Это также работает с объектами. Важно отметить, что оператор Spread не может расширять значения объектных литералов, но 
мы можем использовать его для клонирования свойств из одного объекта в другой:

```javascript
const name = {firstName:"John", lastName:"Doe"};
const hobbies = { hobby1: "singing", hobby2: "dancing" }
const myInfo = {...name, ...hobbies};

console.log(myInfo); //{firstName:"John", lastName:"Doe", hobby1: "singing", hobby2: "dancing"}
```
Подробнее об остаточных параметрах и операторах расширения можно прочитать [здесь](https://learn.javascript.ru/rest-parameters-spread-operator).

### Конструктор Set()

Конструктор Set позволяет создавать объекты Set, в которых хранятся уникальные значения любого типа, будь то примитивные 
значения или ссылки на объекты.

Допустим, у нас есть массив объектов, который мы хотим перебрать для вывода только определенных значений:

```javascript
const animals = [
  {
    name:'Lion',
    category: 'carnivore'
  },
  {
    name:'dog',
    category:'pet'
  },
  {
    name:'cat',
    category:'pet'
  },
  {
    name:'wolf',
    category:'carnivore'
  }
];
const category = animals.map((animal)=>animal.category);

console.log(category); //["carnivore" , "pet" , "pet" , "carnivore"]
```
Новый массив оказался наполнен повторяющимися значениями. Мы могли бы установить условие, чтобы избежать повторения. Но 
конструктор Set() с этим прекрасно справляется:

```javascript
const category = [...new Set(animals.map((animal)=>animal.category))];

console.log(category); ////["carnivore" , "pet"];
```
### Динамические ключи объектов

В JavaScript мы знаем, что объекты часто состоят из свойств / ключей и значений, и мы можем использовать точечную нотацию 
для добавления, редактирования или доступа к некоторым значениям:

```javascript
const lion = {
  category: "carnivore"
};

console.log(lion); // { category: "carnivore" }

lion.baby = 'cub';

console.log(lion.category); // carnivore
console.log(lion); // { category: "carnivore" , baby: "cub" }
```
Динамические ключи — это ключи, которые могут не соответствовать стандартному соглашению об именовании свойств / ключей в
объекте. Стандартное соглашение об именовании допускает только camelCase и snake_case, но, используя обозначения в квадратных 
скобках, мы можем решить эту проблему.

Например, предположим, что мы называем наш ключ с тире между словами, например ( lion-baby):

```javascript
const lion = {
  'lion-baby' : "cub"
};

// dot notation
console.log(lion.lion-baby); // error: ReferenceError: baby is not defined
// bracket notation
console.log(lion['lion-baby']); // "cub"
```
### Метод массива Reduce()

Это, пожалуй, самый мощный метод массива. Когда вы объединяете map() и filter() в цепочку, вы в итоге выполняете работу
дважды – сначала фильтруете каждое отдельное значение, а затем сопоставляете остальные значения. С другой стороны, reduce() 
позволяет фильтровать и сопоставлять данные за один проход. Этот метод является мощным, но в то же время он немного более 
сложный и хитрый.

Мы перебираем наш массив, а затем получаем функцию обратного вызова, которая похожа на map(), filter(), find(), и другие. 
Основное отличие заключается в том, что он сводит наш массив к одному значению, которое может быть числом, массивом или 
объектом.

Еще одна вещь, которую следует иметь в виду при использовании метода reduce(), заключается в том, что мы передаем в него 
два аргумента. Первый аргумент — это сумма всех вычислений, а второй - текущее значение итерации (которое вы вскоре поймете).

Например, предположим, что у нас есть список зарплат для наших сотрудников: 

```javascript
const staffs = [
  { name: "Susan", age: 14, salary: 100 },
  { name: "Daniel", age: 16, salary: 120 },
  { name: "Bruno", age: 56, salary: 400 },
  { name: "Jacob", age: 15, salary: 110 },
  { name: "Sam", age: 64, salary: 500 },
  { name: "Dave", age: 56, salary: 380 },
  { name: "Neils", age: 65, salary: 540 }
];
```
И мы хотим посчитать 10%-ную часть для всего персонала. Мы могли бы легко сделать это с помощью метода сокращения, но, 
прежде чем сделать это, давайте сделаем кое-что попроще: подсчитаем общую зарплату:

```javascript
const totalSalary = staffs.reduce((total, staff) => {
  total += staff.salary;
  
  return total;
},0);

console.log(totalSalary); // 2150
```
Мы передали второй аргумент, который является итогом, это может быть что угодно – например, число или объект. 

Давайте теперь подсчитаем 10%-ную часть для всего персонала и получим общую сумму: 

```javascript
const salaryInfo = staffs.reduce(
  (total, staff) => {
    const staffTithe = staff.salary * 0.1;

    total.totalTithe += staffTithe;
    total.totalSalary += staff.salary;

    return total;
  },
  { totalSalary: 0, totalTithe: 0 }
);

console.log(salaryInfo); // { totalSalary: 2150 , totalTithe: 215 }
```
В данном случае мы использовали объект в качестве второго аргумента. 

Подробнее об методе reduce() можно прочитать [здесь](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce).

### Оператор опциональной последовательности ?.

Оператор опциональной последовательности __?.__ — это безопасный способ доступа к вложенным свойствам объекта в JavaScript
вместо того, чтобы выполнять многократные проверки на нуль при доступе к длинной цепочке свойств объекта. Это новая функция,
представленная в ES2020:

Например: 

```javascript
const users = [
{
    name: "Sam",
    age: 64,
    hobby: "cooking",
    hobbies: {
      hobb1: "cooking",
      hobby2: "sleeping"
    }
  },
  { name: "Bruno", age: 56 },
  { name: "Dave", age: 56, hobby: "Football" },
  {
    name: "Jacob",
    age: 65,
    hobbies: {
      hobb1: "driving",
      hobby2: "sleeping"
    }
  }
];
```

Предположим, вы пытаетесь получить хобби из приведенного выше массива. Давайте попробуем это сделать: 

```javascript
users.forEach((user) => {
  console.log(user.hobbies.hobby2);
});
```
Когда вы заглянете в свою консоль, вы заметите, что первая итерация была завершена, но у второй итерации не было хобби.
Поэтому forEach() пришлось выдать ошибку и прервать итерацию, так как он не мог получать данные от других объектов в массиве: 

```shell
"sleeping"
error: Uncaught TypeError: user.hobbies is undefined
```
Эта ошибка может быть исправлена с помощью опциональной последовательности. Рассмотрим, как мы можем сделать, не прибегая
к этому оператору: 

```javascript
users.forEach((user) => {
  console.log(user.hobbies && user.hobbies.hobby2);
});
```
И вариант с оператором опциональной последовательности:

```javascript
users.forEach((user) => {
  console.log(user?.hobbies?.hobby2);
});
```
Результат в консоли: 

```shell
"sleeping"
undefined
undefined
"sleeping"
```
Как мы видим, оператор опциональной последовательности избавляет нас от лишних условий.

Подробнее о операторе опциональной последовательности __?.__ можно прочитать [здесь](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/Optional_chaining).

### Fetch API

Fetch API, как следует из названия, используется для получения данных из API. Это API браузера, который позволяет вам 
использовать JavaScript для выполнения базовых AJAX-запросов (Asynchronous JavaScript And XML). 

Поскольку он предоставляется браузером, вы можете использовать его без необходимости устанавливать или импортировать какие-либо
пакеты или зависимости (например, axios). Его конфигурация довольно проста для понимания. Fetch API предоставляет промис 
по-умолчанию (про промисы мы говорили выше). 

Давайте рассмотрим, как извлекать данные с помощью fetch API. Для примера мы будем использовать бесплатный API, который
содержит тысячи случайных цитат: 

```javascript
fetch("https://type.fit/api/quotes")
  .then((response) => response.json())
  .then((data) => console.log(data))
  .catch((err) => console.log(err));
```
То, что мы здесь сделали, было: 
- Строка 1: мы получили данные из API, который вернул промис 
- Строка 2: Затем мы получили .json() формат данных, который также является промисом 
- Строка 3: Мы получили наши данные, которые теперь возвращают JSON 
- Строка 4: Мы отловили ошибки на случай, если таковые имеются 

Давайте теперь посмотрим, как мы можем обрабатывать ошибки из fetch API. Метод fetch() автоматически выдает ошибку для сетевых 
ошибок, но не для HTTP-ошибок, таких как ответы от 400 до 5xx. 

Хорошая новость заключается в том, что fetch() возвращает response.ok флаг, указывающий, произошел ли сбой запроса или код 
состояния HTTP-ответа находится в диапазоне успешных. 

Попробуем это реализовать: 

```javascript
fetch("https://type.fit/api/quotes")
  .then((response) => {
    if (!response.ok) {
      throw new Error(response.statusText);
    }
    
    return response.json();
  })
  .then((data) => console.log(data))
  .catch((err) => console.log(err));
```
Подробнее о Fetch API можно прочитать [здесь](https://developer.mozilla.org/ru/docs/Web/API/Fetch_API/Using_Fetch).

### Async/Await

Async/Await позволяет нам писать асинхронный код синхронным способом. По факту – это синтаксический сахар для промисов, 
упрощающий их чтение и использование. Async/Await делают асинхронный код более похожим на синхронный/процедурный код, 
который легче понять.

Асинхронная функция всегда возвращает промис. 

Обратите внимание, что у нас всегда есть async перед функцией, и мы можем использовать await только тогда, когда у нас есть 
async. 

Давайте теперь реализуем код Fetch API, над которым мы работали ранее, используя async/await: 

```javascript
const fetchData = async () =>{
  const quotes = await fetch("https://type.fit/api/quotes");
  const response = await quotes.json();

  console.log(response);
}

fetchData();
```
Это гораздо легче читать, не так ли? 

Возможно, вам интересно, как мы можем обрабатывать ошибки с помощью async/await. Для это мы будем использовать ключевые слова
try и catch: 

```javascript
const fetchData = async () => {
  try {
    const quotes = await fetch("https://type.fit/api/quotes");
    const response = await quotes.json();
    
    console.log(response);
  } catch (error) {
    console.log(error);
  }
};

fetchData();
```
Подробнее о Async/Await можно прочитать [здесь](https://developer.mozilla.org/ru/docs/Learn/JavaScript/Asynchronous/Promises).
