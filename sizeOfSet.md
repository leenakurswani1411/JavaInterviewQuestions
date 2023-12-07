# Que: What will be the size of the Set? 
Ans:
```java
public class SetQuestion {
	    public static void main(String[] args) {
	        Student s1 = new Student(1, "John");
	        Student s2 = new Student(1, "John");
	        Set<Student> set = new HashSet<>();
	        set.add(s1);
	        set.add(s2);
	        System.out.println(set.size());
	    }
}
class Student {
	    public int id;
	    public String name;
	    public Student(int id, String name) {
	        this.id = id;
	        this.name = name;
	    }
	    @Override
	    public String toString() {
	        return "Student{" + "id=" + id + ", name='" + name + '\'' + '}';
	    }
}
```
**Explanation:** 
*In the provided code, you have created two instances of the Student class, s1 and s2. Both instances have the same values for id and name. You then add both instances to a HashSet named set. However, since you haven't overridden the Student class's hashCode() and equals() methods, the default implementations from the Object class are used. These default implementations consider two objects equal if and only if they reference the same object in memory.
In your case, even though s1 and s2 have the same values for id and name, they are different objects in memory. Therefore, the HashSet will consider them as distinct elements, and both will be added to the set.
As a result, when you print the size of the set using System.out.println(set.size());, the output will be 2.*
