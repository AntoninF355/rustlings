# Notes from my learning in rust as i progress.

rustc command = compilation
cargo run = build + lance
lancer le code = .\nomduficher (.exe)
cargo check. This command quickly checks your code to make sure it compiles but doesn’t produce an executable:

## Naming conventions
File naming convention snake case : main_program.rs

Rust code uses snake case as the conventional style for function and variable names

#### convention nommage des constantes full maj et undescore
```
const K_CEST_UNE_CONSTANTE: i32 = 45;
println!("constante = {K_CEST_UNE_CONSTANTE}");
```

* Constants are valid for the entire time a program runs, within the scope in which they were declared. 

* This property makes constants useful for values in your application domain that multiple parts of the program might need to know about, such as the maximum number of points any player of a game is allowed to earn, or the speed of light.

* Naming hardcoded values used throughout your program as constants is useful in conveying the meaning of that value to future maintainers of the code. 

* It also helps to have only one place in your code that you would need to change if the hardcoded value needed to be updated in the future.


## Mutable variables
mut ne concerne qu'un type, pas possible de changer, -> si besoin de changer : shadowing

    let mut x = 5;
    println!("La valeur de x est : {x}");
    x = 6;
    println!("La valeur de x est : {x}");





### Shadowing
Creation d'une nouvelle variable avec le même nom que la précédente, mais avec une nouvelle valeur, et un type différent si besoin.

    let c = 5;
    let c = c + 1;

    {
        let c = c * 2;
        println!("Valeur de c du inner scope = {c}");
    }

    println!("Valeur de c = {c}");


### Fonction avec retour
Mettre le type de retour après les paramètres de la fonction : ->
 
    fn sale_price(price: i64) -> i64 {
        price / 2
    }

Pas besoins de ; pour un retour de resultat implicite (function 5)

Est-ce pour toutes les fonctions ?
Oui, la règle “dernière expression sans ; = valeur retournée” s’applique à toutes les fonctions et tous les blocs (if, match, etc.).

Ca doit correspondre au type de retour de la fonction
return explicite est possible aussi

### Functions

Rust doesn’t care where you define your functions, only that they’re defined somewhere in a scope that can be seen by the caller.

In function signatures, you must declare the type of each parameter

Statements are instructions that perform some action and do not return a value. (ex: declaration de variable ou de fonction : let x = 5; )
Expressions evaluate to a resultant value.  

In those languages, you can write x = y = 6 and have both x and y have the value 6; that is not the case in Rust.

Expressions evaluate to a value and make up most of the rest of the code that you’ll write in Rust. Consider a math operation, such as 5 + 6, which is an expression that evaluates to the value 11. Expressions can be part of statements: In Listing 3-1, the 6 in the statement let y = 6; is an expression that evaluates to the value 6. Calling a function is an expression. Calling a macro is an expression. A new scope block created with curly brackets is an expression.

Une expression n'a pas de point virgule à la fin, sinon elle devient un statement et ne retourne plus de valeur.



### If
Using too many else if expressions can clutter your code, so if you have more than one, you might want to refactor your code. Chapter 6 describes a powerful Rust branching construct called **match** for these cases.


### Arrays

[1;150] -> array de 150 éléments, tous initialisés à 1

### Slice
Partie d'un tableau / string
On note les **index** de la partie que l'on veut récupérer : [start_index..end_index] -> start_index inclus, end_index exclu
On peut inclure l'index de fin avec : [start_index..=end_index]

    let a = [1, 2, 3, 4, 5];
    let slice = &a[1..3]; 


### Tuples
Un tuple est un groupe de valeurs de types différents regroupées en une seule valeur. Les tuples ont une longueur fixe : une fois déclarés, ils ne peuvent pas grandir ou rétrécir en taille.
    let t = (500, 6.4, 1);
    let (x, y, z) = t; // destructuring

Ca décompose en plusieurs variables.

On peut aussi accéder à un élément d'un tuple en utilisant un point suivi de l'index de l'élément souhaité. Les index commencent à zéro.

    let t = (500, 6.4, 1);
    let five_hundred = t.0;
    let six_point_four = t.1;
    ... etc

### Vectors

Collecction, structure de données qui stocke les valeurs les unes à côté des autres dans la mémoire. Uniquement le même type.

    fn main() {
        let v: Vec<i32> = Vec::new();
    }

En général utiliser "Vec< T >", Rust trouvera le type avec ce qui est donné en valeur

Plus pratique, la macro vec! :

    let v = vec![1, 2, 3];


Update un vector : 
Méthode avec fonction push

    output.push(2 * element);

Iteration avec une map

    // Advanced note: This method is more efficient because it automatically
    // preallocates enough capacity. This can be done manually in `vec_loop`
    // using `Vec::with_capacity(input.len())` instead of `Vec::new()`.
    input.iter().map(|element| 2 * element).collect()


## Ownership

### Stack and Heap

#### Stack (pile de plateau)
stack and the heap are parts of memory available to your code to use at runtime, 
The stack stores values in the order it gets them and removes the values in the opposite order. Last in, first out (LIFO). 
Think of a stack of plates: When you add more plates, you put them on top of the pile, and when you need a plate, you take one off the top. 
Adding data is called pushing onto the stack, and removing data is called popping off the stack. 

All data stored on the stack must have a known, fixed size. Data with an unknown size at compile time or a size that might change must be stored on the heap instead.

#### Heap (Restaurant cherchant une table libre pour ton groupe)
The heap is less organized: When you put data on the heap, you request a certain amount of space. The memory allocator finds an empty spot in the heap that is big enough, marks it as being in use, and returns a pointer, which is the address of that location. This process is called allocating on the heap and is sometimes abbreviated as just allocating (pushing values onto the stack is not considered allocating). Because the pointer to the heap is a known, fixed size, you can store the pointer on the stack, but when you want the actual data, you must follow the pointer. Think of being seated at a restaurant. When you enter, you state the number of people in your group, and the host finds an empty table that fits everyone and leads you there. If someone in your group comes late, they can ask where you’ve been seated to find you.



Ajouter au stack est plus rapide, car heap fait de la recherche d'espace libre
La recherche dans la heap est plus lente car on suit un pointeur à la mémoire


### Les règles ownership

- Each value in Rust has an owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.


### Type String

différent du type string literal, exemple let s = "ravuuuus"
let s = String::from("Ravuuus"); type string, créer depuis un string literal

String utile pour une chaine qu'on ne connait pas à l'avance et à stocker, c'est gerer par le heap

string mutable
    
    let mut s = String::from("hello");

    s.push_str(", world!");

