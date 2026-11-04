![[Attachments/Pasted image 20261104143737.png]]
![[Attachments/Pasted image 20261104143837.png]] 

now the explanation,
- import re is importing inbuilt regular expression module.
- \b is used so that only those values whose before and after is not there are changed, or else every substring will be changed.
- parts=line.split("//",1) here 1 is used to say that only consider the first occurance of the slashes and not the other ones. so parts will be a list with parts[0] being the code part and parts[1] being the comment.
- as cue's default arg are started by * we return if that is present.
- ^ is start of string and $ is end of string
- \s is for whitespace \s* is used to match 0 or more occurances.
- text.splitlines(keepends=True) this is used to keep split the whole file into lines and then keep the line endings as it was in the previous file.

**There is commment next to each line in the app, and you can understand it, it is in the downloads folder also**

The file ran locally but in the orky repo there are certain conditions where there is no default value provided and we need to give only the mentioned values in the field which is failing, and other is the mismatch of some peerprotocol value where one is empty and other should be one of the protocols. another one faced was there is type and bool and a *  and next to that it is importing some topology which is not getting the value.