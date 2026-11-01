
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
- instead of using nested fields like json, that is brackets inside brackets, cue uses 