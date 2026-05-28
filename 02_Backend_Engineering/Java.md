---
created: 2026-05-24 13:38
updated: 2026-05-24 13:38
tags:
  - backend
status: active
language:
source:
related:
url:
---


> [!info]
> Backend engineering concept or implementation pattern.

### Anonymous class
 - Class without a name
 - Used for quick definition of class where reusability is not required
 - Ex: 
  ```java
  Comparator<String> rev = new Comparator<String>() {
  /* Anonymous class */
	  @Override
	  public int compare(String s1, String s2) {
		  return s2.compareTo(s1);
	   }
  }
  ```

### Access Modifier

![[Pasted image 20260524134518.png]],
#### Final modifier 
- Immutability to a member variable
- Immutability to class Ex: String, Integer are final classes. Their values cannot be changed once created.

### Collections

![[collections java.png]]