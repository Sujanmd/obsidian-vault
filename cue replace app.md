![[Attachments/Pasted image 20261104143737.png]]
![[Attachments/Pasted image 20261104143837.png]] 

now the explanation,
- import re is importing inbuilt regular expression module.
- \b is used so that only those values whose before and after is not there are changed, or else every substring will be changed.
- parts=line.split("//",1) here 1 is used to say that only consider the first occurance of the slashes and not the other ones. so parts will be a list with parts[0] being the code part and parts[1] being the comment.
- as cue's default arg are started by * we return if that is present.
- ^ is start of string and $ is end of string
- \s is for whitespace \s* is used to match 0 or more occurances.
- 