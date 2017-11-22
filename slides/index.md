- title : React Native with F#
- description : Introduction to React Native with F#
- author : Steffen Forkmann
- theme : night
- transition : default

***

## Elm language introduction

<br />
<br />

### different approach to websites development

<br />
<br />
Krzyś Jaworski - [@whitebear-gh](https://github.com/whitebear-gh/Elm-introduction-presentation/)

Inspired by: 
* [Why Elm?](https://www.youtube.com/watch?v=rU-W6557Dos&t=139s)
* [Introduction to ELM](https://www.youtube.com/watch?v=vgsckgtVdoQ)
* [Elm with F#](https://twitter.com/sforkmann)

***

### Current patterns: 

* MVC
* MVVM
* MV - whatever

---

### Two way binding
### Dirty checks... 

 <img src="images/elm-two-way-data-binding-angularjs.jpg" style="background: transparent; border-style: none;"  width=500 />

---

### observables....

* At some points there was even Object.observe added to browsers...

---

## Nowadays frameworks prefer one-way bindnings

* React
* Angular 2+
* ....

*** 

## So, what is the issue here?

* MVC works quite nice on the servers with stateless HTTP requests 
* But in the browser... Data changes constantly, a lot of refreshes etc (similar to desktop apps like WPF)


---

## State of the application

* Apps in browser are not stateless
* Single Source of truth
* One big JS object with (?)

---

## Hard to track where changes were done

 <img src="images/elm-angular-code.png" style="background: white;" width=500 />

---

## Immutability 

* To avoid two way changes
* thread safe
* ...


***



## ELM

### Soo... What is ELM ?

* Purely functional functional language for front-end web development


---

## Functional ????

#### .... ->


---

## Programming languages 


| Functional  | Imperative  |
|:-:|:-:|
| Series of expressions   | series of statements  |
| Immutable state | mutable state |
| purity ( same inputs -> same output)  | side effects |

--- 

## Yeah... but why ELM ?? 

### What is so unique in it?

* Functional but beginner friendly 
    - Easy to understand concepts / learn
    - Nice introduction to functional world
* Statically typed
* Immutable data
* Funtional types system - no null
* No runtime excpetions
* Consistent management of state (famous ELM architecture)
* Interoperability with JavaScript
    * compiles to JS
* ELM time debugger
* ...

---

## Ok, Ok, but... functional?!

---

### Functions

```elm
add1: number -> number
add1 x = x + 1

add: number -> number -> number
add x y = x + y
```

---

### Partial Application

```elm
add: number -> number -> number
add x y = x + y

add2: number -> number
add2 = add 2

-- add2 y = add 2 y
```

---

### Function Composition

```elm
isEven: Int -> Bool
isEven n = n % 2 ==0

-- (<<) (b -> c) -> (a -> b) -> a -> c
-- not: Bool -> Bool

isOdd: Int -> Bool
isOdd: not << isEven
```

---

### Function Application

* pipe operator

```elm
(|>) : a -> (a -> b) -> b
-- x |> f == f x 

func: List number -> String
func list = list
                |> List.filter (\x -> x > 2)
                |> List.length
                |> toString
-- func [1,2,3,4,5] == "3"

-- alternatively: 
func = toString << List.length << List.filter (\x -> x > 2)
```

---

### Pattern matching

```elm
listSum: List number -> number
listSum: list =
    case list of 
        [] -> 0
        x::xs -> x + listSum xs

listLength: List a -> number
listLength: list =
    case list of 
        [] -> 0
        _::xs -> 1 + listLength xs
```
---

### Functions as first class values

```elm
listFilter: (a -> Bool) -> List a -> List a
listFilter: fn list =
    case list of 
        [] -> []
        x::xs -> 
            f fn x then x::(listFilter fn xs)
            else listFilter fn xs
```
---

### Type Aliases

```elm
type alias UserName = String
type alias UserId = Int
type alias User = 
    { name: UserName
    , id: IserId
    }
-- helpful with designing by types
```

---

### Union Types

```elm
type Msg 
    = Increment
    | Decrement
    | SetValue Int
```

---

### Tuples

```elm
myTuple = ("A", "B", "C")
myNestedTuple = ("A", "B", "C", ("X", "Y", "Z"))

let
  (a,b,c) = myTuple
in 
  a ++ b ++ c
-- "ABC" : String

let
  (a,b,c,(x,y,z)) = myNestedTuple
in
  a ++ b ++ c ++ x ++ y ++ z
-- "ABCXYZ" : String
```

---

### Tuples - pattern matching

```elm
isOrdered : (String, String, String) -> String
isOrdered tuple =
 case tuple of
  ("A","B","C") as orderedTuple ->
    toString orderedTuple ++ " is an ordered tuple."
    
  (_,_,_) as unorderedTuple ->
    toString unorderedTuple ++ " is an unordered tuple."


> isOrdered myTuple
-- "(\"A\",\"B\",\"C\") is an ordered tuple."

> isOrdered ("B", "C", "A")
-- "(\"B\",\"C\",\"A\") is an unordered tuple."
```

---

### Examples ...

```elm
type Visibility = All | Active | Completed

> All
All : Visibility

> Active
Active : Visibility

> Completed
Completed : Visibility
```

---

### ... Examples ...

```elm
type alias Task = { task : String, complete : Bool }

buy : Task
buy =
  { task = "Buy milk", complete = True }

drink : Task
drink =
  { task = "Drink milk", complete = False }

tasks : List Task
tasks =
  [ buy, drink ]

-- keep : Visibility -> List Task -> List Task

-- keep All tasks == [buy,drink]
-- keep Active tasks == [drink]
-- keep Complete tasks == [buy]
```

---

### ... Examples

```elm
type Visibility = All | Active | Completed
type alias Task = { task : String, complete : Bool }

filterTasks : Visibility -> List Task -> List Task
filterTasks visibility tasks =
  case visibility of
    All ->
      tasks

    Active ->
      List.filter (\task -> not task.complete) tasks

    Completed ->
      List.filter (\task -> task.complete) tasks
```

*** 

### Model - View - Update

### "Elm - Architecture"

 <img src="images/Elm.png" style="background: white;" width=700 />


 <small>http://danielbachler.de/2016/02/11/berlinjs-talk-about-elm.html</small>


---

### Model - View - Update


```elm
type alias Model = (...) -- record type

type Msg = (...) -- union

update : Msg -> Model -> (Model, Cmd Msg)
update msg model = (...)

view : Model -> Html Msg
view model = (...)
```
---

### Model - View - Update


```elm
type Msg 
    = Increment 
    | Decrement

update msg model =
  case msg of
    Increment ->
      model + 1

    Decrement ->
      model - 1

```
---

### Model - View - Update


```elm
view model =
  div []
    [ button [ onClick Decrement ] [ text "-" ]
    , div [] [ text (toString model) ]
    , button [ onClick Increment ] [ text "+" ]
    ]
```

<br/>
<br/>

---

### Virtual DOM - Initial

<br />
<br />


 <img src="images/onchange_vdom_initial.svg" style="background: white;" />

<br />
<br />

 <small>http://teropa.info/blog/2015/03/02/change-and-its-detection-in-javascript-frameworks.html</small>

---

### Virtual DOM - Change

<br />
<br />


 <img src="images/onchange_vdom_change.svg" style="background: white;" />

<br />
<br />

 <small>http://teropa.info/blog/2015/03/02/change-and-its-detection-in-javascript-frameworks.html</small>

---

### Virtual DOM - Reuse

<br />
<br />


 <img src="images/onchange_immutable.svg" style="background: white;" />

<br />
<br />

 <small>http://teropa.info/blog/2015/03/02/change-and-its-detection-in-javascript-frameworks.html</small>


---

### Model - View - Update

# [Demo](http://elm-lang.org/examples/buttons)

***

### TakeAways

* Learn all the FP you can!
* Simple modular design
* .Net fans? F# (Fable) + Elm!  
* [elm-lang.org/try](http://elm-lang.org/try)

*** 

### Thank you!
* https://www.youtube.com/watch?v=vgsckgtVdoQ
* https://gist.github.com/yang-wei/4f563fbf81ff843e8b1e
* https://github.com/fable-compiler/fable-elmish
* https://ionide.io
* https://facebook.github.io/react-native/
