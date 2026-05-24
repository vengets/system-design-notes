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
