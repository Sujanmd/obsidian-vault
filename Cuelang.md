
- cue is like a json based spreadsheet
- cue is a superset of json
- CUE- configure, unify, execute
- Although the language is not a general-purpose programming language, it has many applications, such as data validation, data templating, configuration, querying, code generation and even scripting.
- It merges types and values into single concept.
- used to validate helm files before deployment
- we write it in .cue file, it is like json file only but it has some lenience unlike json where there is no need to add commas after every line and no need to omit a comma after the last element.
- initially all the constraints are written in the config.cue file and then it is evaluated against it.
- `cue eval config.cue`  is used to evaluate the code written.
- Most existing tools (like Kubernetes, Docker Compose, or cloud APIs) don't read `.cue` files directly; they expect JSON or YAML. `cue export` converts your resolved CUE definitions into standard data formats:
  `cue export config.cue`- this exports the file to json.
  `cue export config.cue --out yaml`- to yaml
- **You don't even have to write your data in CUE to benefit from it. You can write your schema in a `.cue` file and use `cue vet` to check existing YAML or JSON files:**
  `cue vet schema.cue deployment.yaml` 
- In a real project, you don't keep everything in one file. You can split schemas and concrete values across files in the same directory:
  `schema.cue` defines the shapes and rules.
  `dev.cue` provides the concrete settings for development.
  `prod.cue` provides the settings for production.
  `cue export schema.cue dev.cue`
- Types are values
- -e flag is used to tell the cli to evaluate an expression
- ![[Attachments/Pasted image 20261101164528.png]]
- ![[Attachments/Pasted image 20261101164557.png]] 
- & is the explicit unification operator in cuelang
- In CUE, schemas are typically written as **definitions**. A definition is a field whose identifier starts with `#` or `_#`.
- In CUE, the **`-c`** (or **`--concrete`**) flag tells commands like `cue eval` or `cue vet` to verify that **every value is completely concrete** (has a specific, final value) and that no unresolved types or open constraints remain.
- ![[Attachments/Pasted image 20261101165852.png|579]] 
- CUE’s core operations are defined so that the order in which configurations are combined is unimportant.  We say that **order is irrelevant** in CUE.
- instead of using nested fields like json, that is brackets inside brackets, cue uses `fruit: grape: weight: 2` directly
- ![[Attachments/Pasted image 20261101170820.png|351]] 
- we can do anything like we do in json or we do in clue, both are valid and works
- ![[Attachments/Pasted image 20261102100406.png|578]] 
- before kubernetes borg was the language used.
- CUE was created to fix **"YAML hell"**—the fragility, errors, and confusion that happen when configuration files for tools like Kubernetes and cloud platforms become massive and complex.
- Specifying a field multiple times with conflicting values results in an error, or **bottom** written `_|_`.
- In the CUE CLI, the **`-i`** flag (used with `cue eval`) tells CUE to **continue evaluating and display errors inline as literal values** instead of halting immediately.
- **Top**, written `_`, represents any CUE value. It can be thought of as a placeholder, waiting to be given a more specific value elsewhere.
- `#"The hazard symbol \#U00002622 is Unicode code point "\U00002622"."#` used for raw string.
- String types are enclosed in double quotes and bytes are in single quotes. Bytes are strored and exported as base64 encoded data where we can specify \xnn(hexa) or \nnn(octal) values.
- CUE’s most important composite type is the **struct**, also known as a **map**. Its members are called **fields**.
- ![[Attachments/Pasted image 20261102120137.png]] 
- ![[Attachments/Pasted image 20261102120700.png]] this is where the "_ | _  " is used .
-  A struct may be open or closed, it is open unless it is closed
- The main use of the **`close()`** built-in function in CUE is to turn an **open struct** into a **closed struct**, preventing any extra, unlisted fields from being added to it.
- no extra fields can be added and unified.
- In CUE, schemas are typically written as **definitions**. A definition is a field whose identifier starts with `#` or `_#`.
- Referring to a definition **closes** any structs and lists recursively. Structs and lists can be left **open** using `...`.
- ![[Attachments/Pasted image 20261102123259.png]]  ![[Attachments/Pasted image 20261102123309.png]]
- `protocol: "tcp" | "udp" | "sctp" | "dccp"` this is disjunction, any one of these values should be selected or given as input, then there wont be any error
- default value is used to select which element in the disjunction should be selected when no value is given for the specified field, it is marked with " * " .
- Disjunctions work for any type, including structs. `[...#Floor]` defines an **open list constraint**: it specifies that `floors` can contain any number of items, but **every element in the list must satisfy the `#Floor` schema**.
- ![[Attachments/Pasted image 20261102131021.png|589]] 
- bounds can be written on our own like greater than or less than and also can be predefined like uint(>=0) where u is used for unsigned numbers. unit8,uint16 is used to say the length of the word that is being used.
- **lists** are sequence where members are called elements.
- ![[Attachments/Pasted image 20261102143627.png]] 
- [...] is used in lists to show that they are of variable length.
- ### templates-
- it is used to specify optional fields in bulk 
- ![[Attachments/Pasted image 20261102144522.png]]  here the name in square brackets [Name= _] is the pattern of the template_ inside which the pattern constraint will be present. it should be either string or _ .
- =~ is the cue's regex match operator. 
- ![[Attachments/Pasted image 20261102150151.png|417]] 
- here the value of the nesting is same as other langs, it matches the one values in the same scope or in the order towards outer most declaration.
- 