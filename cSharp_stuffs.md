# C# stuffs

- ## strings
  - Strings in C sharp are reference type that acts like a value type
  ```c#
   var string1= "something"; var string2=string1; string2="sumtin";         // string1="something" et string2="sumtin"
  ```
- ## lists

  - avoid using ArrayList , it dosen't use generics, it simply casts an object (if its a list of a value type it would box it => waste of performance), use "System.Collections.Generic" collections instead

- ## Covariance and Contravariance

  - in case of delegates, the return type is covariant and the params are contravariant
  - in generics => covariance with out keyword , contravariance with in
  - IEnumerable has covariant type parameter => IEnumerable \<out T > which is why something like this possible : IEnumerable \<Vehicule> vehicules = new List \<Cars >
  - personnal mnemonics :
    - covariance => kb eats sgh
    - contravariance = sgh eats kb
  - example from stackoverflow , CSharper answer :
    https://stackoverflow.com/questions/2662369/covariance-and-contravariance-real-world-example

  ```c#

    public interface ICovariant<out T> { }
    public interface IContravariant<in T> { }

    public class Covariant<T> : ICovariant<T> { }
    public class Contravariant<T> : IContravariant<T> { }

    public class Fruit { }
    public class Apple : Fruit { }

    public class TheInsAndOuts
    {
    public void Covariance()
    {
    ICovariant<Fruit> fruit = new Covariant<Fruit>();
    ICovariant<Apple> apple = new Covariant<Apple>();

            Covariant(fruit);
            Covariant(apple); //apple is being upcasted to fruit, without the out keyword this will not compile
        }

        public void Contravariance()
        {
            IContravariant<Fruit> fruit = new Contravariant<Fruit>();
            IContravariant<Apple> apple = new Contravariant<Apple>();

            Contravariant(fruit); //fruit is being downcasted to apple, without the in keyword this will not compile
            Contravariant(apple);
        }

        public void Covariant(ICovariant<Fruit> fruit) { }

        public void Contravariant(IContravariant<Apple> apple) { }

    }
  ```
