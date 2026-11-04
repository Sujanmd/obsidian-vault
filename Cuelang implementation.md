- In CUE, the `:schema` syntax is required whenever the **package name inside the file differs from the folder name**.
- The name of the package should be same as the name of the folder the package is in. so it wont be a problem or an overhead while importing it.
- `cue eval player.cue appinfra.yaml capsequo.yaml deployment.yaml nodegroups.yaml opsconfig.yaml playerval.yaml plshvalues.yaml workspaceoutput.yaml --out yaml`
- cue vet and cue eval is different remember!!!
# what we got to know
in amagictl the cue files are evaluated in the opp order that we are doing, we are doing bottom to up and there it is done top to bottom. 
so cue eval works but cue export does not work because to export interpolation requires a value and that value to the imported package cannot be provided in a bottom up approach like a yaml file, but the higher file will have access to the lower one. now when we are doing we cannot export, instead we need to evaluate the field thats it! but if we need to i just got a theory that it sends the library that imported file will be validated against the cue file before it is evaluated.






