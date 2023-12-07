## How hashcode is generated? 
**Ans: The hashCode() method in Java is used to calculate a hash code value for an object.**
**HashCode**:*The hash code is a numerical representation of the object's contents and is used by data structures, such as HashMap or HashSet, to quickly locate an object in a collection.*
**Here's a simple explanation of how the default hashCode() method works and how you might implement it:**
*Default Implementation (in Object class): The default hashCode() method in the Object class generates a hash code based on the memory address of the object. Two distinct objects, even if they have the same content, will have different hash codes unless their references point to the same memory location.*

``java
@Override
public int hashCode() {
    return Objects.hash(id, name);
}
```
Here, Objects.hash() is a utility method that combines the hash codes of the specified fields. It internally uses a formula to generate a hash code that takes into account the hash codes of individual fields.

Additionally, it's not required (and often not practical) for the hash code to be unique for every distinct object. Collisions (different objects having the same hash code) are expected, and good hash functions aim to distribute hash codes evenly to reduce the likelihood of collisions.

Remember that when you override equals(), you should always override hashCode() to maintain the contract that equal objects must have equal hash codes. This is important for correct behavior when using objects in hash-based collections like HashSet or HashMap.
