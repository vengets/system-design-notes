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

![[collections java.png|1339]]



### Iterator 

![[Screenshot 2026-05-30 at 5.19.13 PM.png|2106]]d

#### ListIterator can go both directions where Iterator can only goes to next element.


### Comparable<Integer> vs Comparator

#### Use Comparable if you have access to change the class

``` java
class Person implements Comparable<Person> {

 @Override
 public int compareTo(Person personTo) {
   return 0;
  }
}
```

You can use

```
Collections.sort(persons);
```
---

#### //Comparator outside of the Person class

```
class NotInPersonClass implements Comparator<Person> {

@Override
public int compare(Person o1, Person o2){
  return 0;
 }
}
```

You can use
```
Collections.sort(people, new NotInPersonClass());
```
or
```
people.sort(new NotInPersonClass());
```
or
```
people.sort(new Comparator<Person>() {  
@Override  
public int compare(Person p1, Person p2) {  
return Integer.compare(p1.age, p2.age);  
}  
});
```
or Lambda (most common today)

```
people.sort((p1, p2) -> Integer.compare(p1.age, p2.age));
```

Or even shorter:

```
people.sort(Comparator.comparingInt(p -> p.age));
```

```