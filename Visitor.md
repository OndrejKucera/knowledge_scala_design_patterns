## Visitor

Pattern allows to add new operations to an existing data type. Visitor allows to fairly easily add new operations to an existing data type, but it makes adding new implementations of the data type difficult.
 - If we want to add a new operation, we just need to create a new visitor and write code such that it knows how to visit each existing concrete element.
 - It’s hard to add new implementations. To do so, we’d need to modify all of the existing visitors to know how to visit the new data implementation. If those Visitor classes are outside of our control, it may be impossible!
 - Relared patterns: [Decorator](https://github.com/OndrejKucera/knowledge_design_patterns/blob/master/Decorator.md)
 
### Java Example
 ```java
 public interface ItemElement {
   public int accept(ShoppingVisitor visitor);
 }
 
 public class Book implements ItemElement {
   private int price;
   private String isbnNumber;
   ...
   @Override
   public int accept(ShoppingVisitor visitor) {
     return visitor.visit(this);
   }
 }
 
 public class Fruit implements ItemElement {
   private int price;
   private String name;
	  ...
   @Override
   public int accept(ShoppingCartVisitor visitor) {
     return visitor.visit(this);
   }
 }
 
 public interface ShoppingVisitor {
  	int visit(ItemElement element);
 }
 
 public class ShoppingVisitorImpl implements ShoppingVisitor {
   @Override
   public int visit(ItemElement element) {
     element.getPrice()
   }
 }
 
 public class ShoppingClient {
	  public static void main(String[] args) {
    ItemElement[] items = new ItemElement[]{new Book(20, "1234"),new Book(100, "5678"),
        new Fruit(10, "Banana"), new Fruit(5, "Apple")};

    int total = 0;
    ShoppingVisitor visitor = new ShoppingVisitorImpl();
    for(ItemElement item : items){
      total += item.accept(visitor);
    }

    System.out.println("Total Cost = " + total);
	 }
}
 ```

### Scala Replacement
 ```scala
 trait Person {
   def fullName: String
   def firstName: String
   def lastName: String
   def houseNum: Int
   def street: String
 }
 
 class SimplePerson(val firstName: String, val lastName: String,
   val houseNum: Int, val street: String) extends Person {
   def fullName = firstName + " " + lastName
 }
 
 val simplePerson = new SimplePerson("Mike", "Linn", 123, "Fake. St.")
 simplePerson.fullName // Mike Linn
 
 implicit class ExtendedPerson(person: Person) {
   def fullAddress = person.houseNum + " " + person.street
 }
 
 simplePerson.fullAddress    // 123 Fake. St.
 ```