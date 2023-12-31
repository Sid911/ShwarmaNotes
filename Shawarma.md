### Shawarma Specification

Syntax

```rust
// decleration
ing frozen hello : i32 = 5;
ingredient frozen s : string = "Hello";
ing character: char = '5';

// Function decleration
recipe functionName(ingredient1: type, ingredient2: type, ...): returnType {
    // Recipe for the function
    bill 
}

// Conditional checks
taste (condition) {
    // Follow this recipe if the condition is delicious
} elseSpice (condition2) {
    // Try this alternative seasoning if condition2 is flavorful
} swallow {
    // Savor this option if none of the above flavors are satisfactory
}

// Creative syntax for a while loop in Shwarma
stir ( h < r) {
    // Continue cooking while the condition tastes good
}

// Creative syntax for a for loop in Shwarma
bake (recpie i : i32 = 0; i < 10; i++) {
    // Dish out the loop with i ranging from 0 to 9
    //break
    giveUp;
    // continue
    nextPls;
}

favorite (choice) {
    case "pizza":
    serve("Great choice! Our pizza is delicious.");
    digest;

    case "shawarma":
    serve("Ah, a shawarma fan! Our shawarmas are top-notch.");
    digest;

    case "salad":
    case "healthy":
    serve("Opting for a salad? Excellent choice for the health-conscious!");
    digest;

    case "ice cream":
    case "dessert":
    serve("Satisfy your sweet cravings with our delightful ice creams.");
    digest;

    otherwise:
    serve("Sorry, we don't have that item on the menu.");
}

// Print
string serve("Hello, Shwarma!{} {}", variable, variable);
void serve("Hello, Shwarma!{} {}", variable, variable);

serve("") // stdout
string converted = serve("X : {var + var}") // return after convert

// Input
string order("Pls tell your order : ")

// Strcuture Decleration
menu IceCream {
    hello: string;
    c: char;
}

// Enum
flavor ANIMAL{
    DOG = 10,
    CAT = 20,
}

```
### Datatypes

```rust
null
bool
char
string
i8
i16
i32
i64
float16
float32
float64
array: i8[50]
```

### Keywords
- ingredient [ing]    // variable decleration
- frozen        // non mutable
- recipe        // function
- bill          // return
- taste         // if
- elseSpice     // elseif
- swallow       // else
- stir          // while
- bake          // for
- giveUp        // break
- nextPls       // continue
- favorite      // switch
- case          // case
- otherwise     // default
- serve         // println
- order         // input stdin
- menu          // struct
- flavor        // enum
- potato        // trait
- cook          // implementation
- for           // for in implementation `cook [Trait] for [struct]`
- me            // Self for structs
- digest        // gc the memory


Example code

```rust
menu Shawarma {
    name: string;
    type: string;
    ingredients: string[];
    meatType: string;
}
cook Shawarma{
    recipe getMeatType(me): string { bill me.meatType; }
}

potato Food{
    recipe prepare(me, orders: u32): DateTime;
    recipe getName(me): string;
}

cook Food for Shawarma{
    recipe prepare(me, orders: u32): DateTime{
        bill DateTime.now() + ingredients.size() * 100;
    }
}

recipe addNumbers<i32>(frozen a: i32, frozen b: i32): i32 {
    bill a + b;
}

recipe main(){
    ingredient frozen a = 10 , frozen b = 10;
    ing arr: mellow[] = [1, 2, 3, 4, 5, 6];
    
    ing c: string;
    ing = "string";
    c = addNumbers(a, b);
    serve("A: {a} + B: {b} = C : {c}"); // A: 10 + B: 10 = C : 20
    serve("A: ", toString(a), " + B: ", toString(b), " = C : ", toString(c), "\n" ) // expanded for
}
```
