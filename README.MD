# Notes from my learning in rust as i progress.

rustc command = compilation
cargo run = build + lance
lancer le code = .\nomduficher (.exe)
cargo check. This command quickly checks your code to make sure it compiles but doesn’t produce an executable:

## Naming conventions
File naming convention snake case : main_program.rs

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

