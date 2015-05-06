javarevisited.blogspot.com

http://www.javamex.com/tutorials/collections/hashmaps2.shtml

http://www.programminginterview.com/content/linked-lists

aptitude- http://www.indiabix.com/aptitude/permutation-and-combination/

http://docs.oracle.com/javase/tutorial/java/generics/gentypeinference.html

http://docs.oracle.com/javase/7/docs/api/java/util/TreeMap.html

http://docs.oracle.com/javase/tutorial/collections/implementations/map.html

Garbage collection-http://java.sun.com/j2se/reference/whitepapers/memorymanagement_whitepaper.pdf

Collections Interview Questions

Q1) What is difference between ArrayList and vector?

Ans: )

1) Synchronization - ArrayList is not thread-safe whereas Vector is thread-safe. In Vector class each method like add(), get(int i) is surrounded with a synchronized block and thus making Vector class thread-safe.

2) Data growth - Internally, both the ArrayList and Vector hold onto their contents using an Array. When an element is inserted into an ArrayList or a Vector, the object will need to expand its internal array if it runs out of room. A Vector defaults to doubling the size of its array, while the ArrayList increases its array size by 50 percent.

Q2) How can Arraylist be synchronized without using Vector?

Ans) Arraylist can be synchronized using:

Collection.synchronizedList(List list)

Other collections can be synchronized:

Collection.synchronizedMap(Map map)

Collection.synchronizedCollection(Collection c)

Q3) If an Employee class is present and its objects are added in an arrayList. Now I want the list to be sorted on the basis of the employeeID of Employee class. What are the steps?

Ans) 1)	Implement Comparable interface for the Employee class and override the compareTo(Object obj) method in which compare the employeeID

2)	Now call Collections.sort() method and pass list as an argument.

Now consider that Employee class is a jar file.

1)	Since Comparable interface cannot be implemented, create Comparator and override the compare(Object obj, Object obj1) method .

2)	Call Collections.sort() on the list and pass comparator as an argument.

Q4)What is difference between HashMap and HashTable?

Ans) Both collections implements Map. Both collections store value as key-value pairs. The key differences between the two are

1.	Hashmap is not synchronized in nature but hshtable is.

2.	Another difference is that iterator in the HashMap is fail-safe while the enumerator for the Hashtable isn't.
Fail-safe - â€œif the Hashtable is structurally modified at any time after the iterator is created, in any way except through the iterator's own remove method, the iterator will throw a ConcurrentModificationExceptionâ€?

3.	 HashMap permits null values and only one null key, while Hashtable doesn't allow key or value as null.

Q5) What are the classes implementing List interface?

Ans)
There are three classes that implement List interface:
1) ArrayList : It is a resizable array implementation. The size of the ArrayList can be increased dynamically also operations like add,remove and get can be formed once the object is created. It also ensures that the data is retrieved in the manner it was stored. The ArrayList is not thread-safe.

2) Vector: It is thread-safe implementation of ArrayList. The methods are wrapped around a synchronized block.

3) LinkedList: the LinkedList also implements Queue interface and provide FIFO(First In First Out) operation for add operation. It is faster if than ArrayList if it performs insertion and deletion of elements from the middle of a list.
Q6) Which all classes implement Set interface?

Ans) A Set is a collection that contains no duplicate elements. More formally, sets contain no pair of elements e1 and e2 such that e1.equals(e2), and at most one null element. HashSet,SortedSet and TreeSet are the commnly used class which implements Set interface.

SortedSet - It is an interface which extends Set. A the name suggest , the interface allows the data to be iterated in the ascending order or sorted on the basis of Comparator or Comparable interface. All elements inserted into the interface must implement Comparable or Comparator interface.

TreeSet - It is the implementation of SortedSet interface.This implementation provides guaranteed log(n) time cost for the basic operations (add, remove and contains). The class is not synchronized.

HashSet: This class implements the Set interface, backed by a hash table (actually a HashMap instance). It makes no guarantees as to the iteration order of the set; in particular, it does not guarantee that the order will remain constant over time. This class permits the null element. This class offers constant time performance for the basic operations (add, remove, contains and size), assuming the hash function disperses the elements properly among the buckets

Q7) What is difference between List and a Set?

Ans)
1) List can contain duplicate values but Set doesnt allow. Set allows only to unique elements.
2) List allows retrieval of data to be in same order in the way it is inserted but Set doesnt ensures the sequence in which data can be retrieved.(Except HashSet)

Q8) What is difference between Arrays and ArrayList ?

Ans) Arrays are created of fix size whereas ArrayList is of not fix size. It means that once array is declared as :

int [.md](.md) intArray= new int[6](6.md);
intArray[7](7.md)   // will give ArraysOutOfBoundException.
Also the size of array cannot be incremented or decremented. But with arrayList the size is variable.
Once the array is created elements cannot be added or deleted from it. But with ArrayList the elements can be added and deleted at runtime.
List list = new ArrayList();
list.add(1);
list.add(3);
list.remove(0) // will remove the element from the 1st location.
ArrayList is one dimensional but array can be multidimensional.
> int[.md](.md)[.md](.md)[.md](.md) intArray= new int[3](3.md)[2](2.md)[1](1.md);   // 3 dimensional array

To create an array the size should be known or initalized to some value. If not initialized carefully there could me memory wastage. But arrayList is all about dynamic creation and there is no wastage of memory.
Q9) When to use ArrayList or LinkedList ?

Ans)  Adding new elements is pretty fast for either type of list. For the ArrayList, doing  random lookup using "get" is fast, but for LinkedList, it's slow. It's slow because there's no efficient way to index into the middle of a linked list. When removing elements, using ArrayList is slow. This is because all remaining elements in the underlying array of Object instances must be shifted down for each remove operation. But here LinkedList is fast, because deletion can be done simply by changing a couple of links. So an ArrayList works best for cases where you're doing random access on the list, and a LinkedList works better if you're doing a lot of editing in the middle of the list.

Source : Read More - from java.sun

Q10) Consider a scenario. If an ArrayList has to be iterate to read data only, what are the possible ways and which is the fastest?

Ans) It can be done in two ways, using for loop or using iterator of ArrayList. The first option is faster than using iterator. Because value stored in arraylist is indexed access. So while accessing the value is accessed directly as per the index.

Q11) Now another question with respect to above question is if accessing through iterator is slow then why do we need it and when to use it.

Ans) For loop does not allow the updation in the array(add or remove operation) inside the loop whereas Iterator does. Also Iterator can be used where there is no clue what type of collections will be used because all collections have iterator.

Q12) Which design pattern Iterator follows?

Ans) It follows Iterator design pattern. Iterator Pattern is a type of behavioral pattern. The Iterator pattern is one, which allows you to navigate through a collection of data using a common interface without knowing about the underlying implementation. Iterator should be implemented as an interface. This allows the user to implement it anyway its easier for him/her to return data. The benefits of Iterator are about their strength to provide a common interface for iterating through collections without bothering about underlying implementation.

Example of Iteration design pattern - Enumeration The class java.util.Enumeration is an example of the Iterator pattern. It represents and abstract means of iterating over a collection of elements in some sequential order without the client having to know the representation of the collection being iterated over. It can be used to provide a uniform interface for traversing collections of all kinds.

Q12) Is it better to have a HashMap with large number of records or n number of small hashMaps?

Ans) It depends on the different scenario one is working on:
1) If the objects in the hashMap are same then there is no point in having different hashmap as the traverse time in a hashmap is invariant to the size of the Map.
2) If the objects are of different type like one of Person class , other of Animal class etc then also one can have single hashmap but different hashmap would score over it as it would have better readability.
Q13) Why is it preferred to declare: List

&lt;String&gt;

 list = new ArrayList

&lt;String&gt;

(); instead of ArrayList

&lt;String&gt;

 = new ArrayList

&lt;String&gt;

();

Ans) It is preferred because:

If later on code needs to be changed from ArrayList to Vector then only at the declaration place we can do that.
The most important one – If a function is declared such that it takes list. E.g void showDetails(List list);
When the parameter is declared as List to the function it can be called by passing any subclass of List like ArrayList,Vector,LinkedList making the function more flexible
Q14)	What is difference between iterator access and index access?

Ans) Index based access allow access of the element directly on the basis of index. The cursor of the datastructure can directly goto the 'n' location and get the element. It doesnot traverse through n-1 elements.

In Iterator based access, the cursor has to traverse through each element to get the desired element.So to reach the 'n'th element it need to traverse through n-1 elements.

Insertion,updation or deletion will be faster for iterator based access if the operations are performed on elements present in between the datastructure.

Insertion,updation or deletion will be faster for index based access if the operations are performed on elements present at last of the datastructure.

Traversal or search in index based datastructure is faster.

ArrayList is index access and LinkedList is iterator access.

Q15) How to sort list in reverse order?

Ans) To sort the elements of the List in the reverse natural order of the strings, get a reverse Comparator from the Collections class with reverseOrder(). Then, pass the reverse Comparator to the sort() method.

List list = new ArrayList();

Comparator comp = Collections.reverseOrder();

Collections.sort(list, comp)

Q16) Can a null element added to a Treeset or HashSet?

Ans) A null element can be added only if the set contains one element because when a second element is added then as per set defination a check is made to check duplicate value and comparison with null element will throw NullPointerException.
HashSet is based on hashMap and can contain null element.

Q17) How to sort list of strings - case insensitive?

Ans) using Collections.sort(list, String.CASE\_INSENSITIVE\_ORDER);

Q18) How to make a List (ArrayList,Vector,LinkedList) read only?

Ans) A list implemenation can be made read only using Collections.unmodifiableList(list). This method returns a new list. If a user tries to perform add operation on the new list; UnSupportedOperationException is thrown.

Q19) What is ConcurrentHashMap?

Ans) A concurrentHashMap is thread-safe implementation of Map interface. In this class put and remove method are synchronized but not get method. This class is different from Hashtable in terms of locking; it means that hashtable use object level lock but this class uses bucket level lock thus having better performance.

Q20) Which is faster to iterate LinkedHashSet or LinkedList?

Ans) LinkedList.

Q21) Which data structure HashSet implements

Ans) HashSet implements hashmap internally to store the data. The data passed to hashset is stored as key in hashmap with null as value.

Q22) Arrange in the order of speed - HashMap,HashTable, Collections.synchronizedMap,concurrentHashmap

Ans) HashMap is fastest, ConcurrentHashMap,Collections.synchronizedMap,HashTable.

Q23) What is identityHashMap?

Ans) The IdentityHashMap uses == for equality checking instead of equals(). This can be used for both performance reasons, if you know that two different elements will never be equals and for preventing spoofing, where an object tries to imitate another.

Q24) What is WeakHashMap?

Ans) A hashtable-based Map implementation with weak keys. An entry in a WeakHashMap will automatically be removed when its key is no longer in ordinary use. More precisely, the presence of a mapping for a given key will not prevent the key from being discarded by the garbage collector, that is, made finalizable, finalized, and then reclaimed. When a key has been discarded its entry is effectively removed from the map, so this class behaves somewhat differently than other Map implementations.



How HashMap works in Java
How HashMap works in Java or sometime how get method work in HashMap is common interview questions now days. Almost everybody who worked in Java knows what hashMap is, where to use hashMap or difference between hashtable and HashMap then why this interview question becomes so special? Because of the breadth and depth this question offers. It has become very popular java interview question in almost any senior or mid-senior level java interviews.

Questions start with simple statement

"Have you used HashMap before" or "What is HashMap? Why do we use it “
Almost everybody answers this with yes and then interviewee keep talking about common facts about hashMap like hashMap accpt null while hashtable doesn't, HashMap is not synchronized, hashMap is fast and so on along with basics like its stores key and value pairs etc.
This shows that person has used hashMap and quite familiar with the functionality HashMap offers but interview takes a sharp turn from here and next set of follow up questions gets more detailed about fundamentals involved in hashmap. Interview here you and come back with questions like

"Do you Know how hashMap works in Java” or
"How does get () method of HashMap works in Java"
And then you get answers like I don't bother its standard Java API, you better look code on java; I can find it out in Google at any time etc.
But some interviewee definitely answer this and will say "HashMap works on principle of hashing, we have put () and get () method for storing and retrieving data from hashMap. When we pass an object to put () method to store it on hashMap, hashMap implementation calls
hashcode() method hashMap key object and by applying that hashcode on its own hashing funtion it identifies a bucket location for storing value object , important part here is HashMap stores both key+value in bucket which is essential to understand the retrieving logic. if people fails to recognize this and say it only stores Value in the bucket they will fail to explain the retrieving logic of any object stored in HashMap . This answer is very much acceptable and does make sense that interviewee has fair bit of knowledge how hashing works and how HashMap works in Java.
But this is just start of story and going forward when depth increases a little bit and when you put interviewee on scenarios every java developers faced day by day basis. So next question would be more likely about collision detection and collision resolution in Java HashMap e.g

"What will happen if two different objects have same hashcode?”
Now from here confusion starts some time interviewer will say that since Hashcode is equal objects are equal and HashMap will throw exception or not store it again etc. then you might want to remind them about equals and hashCode() contract that two unequal object in Java very much can have equal hashcode. Some will give up at this point and some will move ahead and say "Since hashcode () is same, bucket location would be same and collision occurs in hashMap, Since HashMap use a linked list to store in bucket, value object will be stored in next node of linked list." great this answer make sense to me though there could be some other collision resolution methods available this is simplest and HashMap does follow this.
But story does not end here and final questions interviewer ask like

"How will you retreive if two different objects have same hashcode?”
> Hmmmmmmmmmmmmm
Interviewee will say we will call get() method and then HashMap uses keys hashcode to find out bucket location and retrieves object but then you need to remind him that there are two objects are stored in same bucket , so they will say about traversal in linked list until we find the value object , then you ask how do you identify value object because you don't value object to compare ,So until they know that HashMap stores both Key and Value in linked list node they won't be able to resolve this issue and will try and fail.

But those bunch of people who remember this key information will say that after finding bucket location , we will call keys.equals() method to identify correct node in linked list and return associated value object for that key in Java HashMap. Perfect this is the correct answer.

In many cases interviewee fails at this stage because they get confused between hashcode () and equals () and keys and values object in hashMap which is pretty obvious because they are dealing with the hashcode () in all previous questions and equals () come in picture only in case of retrieving value object from HashMap.
Some good developer point out here that using immutable, final object with proper equals () and hashcode () implementation would act as perfect Java HashMap keys and improve performance of Java hashMap by reducing collision. Immutability also allows caching there hashcode of different keys which makes overall retrieval process very fast and suggest that String and various wrapper classes e.g Integer provided by Java Collection API are very good HashMap keys.

Now if you clear all this java hashmap interview question you will be surprised by this very interesting question "What happens On HashMap in Java if the size of the Hashmap exceeds a given threshold defined by load factor ?". Until you know how hashmap works exactly you won't be able to answer this question.
if the size of the map exceeds a given threshold defined by load-factor e.g. if load factor is .75 it will act to re-size the map once it filled 75%. Java Hashmap does that by creating another new bucket array of size twice of previous size of hashmap, and then start putting every old element into that new bucket array and this process is called rehashing because it also applies hash function to find new bucket location.

If you manage to answer this question on hashmap in java you will be greeted by "do you see any problem with resizing of hashmap in Java" , you might not be able to pick the context and then he will try to give you hint about multiple thread accessing the java hashmap and potentially looking for race condition on HashMap in Java.

So the answer is Yes there is potential race condition exists while resizing hashmap in Java, if two thread at the same time found that now Java Hashmap needs resizing and they both try to resizing. on the process of resizing of hashmap in Java , the element in bucket which is stored in linked list get reversed in order during there migration to new bucket because java hashmap doesn't append the new element at tail instead it append new element at head to avoid tail traversing. if race condition happens then you will end up with an infinite loop. though this point you can potentially argue that what the hell makes you think to use HashMap in multi-threaded environment to interviewer :)

I like this question because of its depth and number of concept it touches indirectly, if you look at questions asked during interview this HashMap questions has verified
Concept of hashing
Collision resolution in HashMap
Use of equals () and hashCode () method and there importance?
Benefit of immutable object?
race condition on hashmap in Java
Resizing of Java HashMap

Just to summarize here are the answers which does makes sense for above questions

How HashMAp works in Java
HashMap works on principle of hashing, we have put () and get () method for storing and retrieving object form hashMap.When we pass an both key and value to put() method to store on HashMap, it uses key object hashcode() method to calculate hashcode and they by applying hashing on that hashcode it identifies bucket location for storing value object.
While retrieving it uses key object equals method to find out correct key value pair and return value object associated with that key. HashMap uses linked list in case of collision and object will be stored in next node of linked list.
Also hashMap stores both key+value tuple in every node of linked list.

What will happen if two different HashMap key objects have same hashcode?
They will be stored in same bucket but no next node of linked list. And keys equals () method will be used to identify correct key value pair in HashMap.

In terms of usage HashMap is very versatile and I have mostly used hashMap as cache in electronic trading application I have worked . Since finance domain used Java heavily and due to performance reason we need caching a lot HashMap comes as very handy there.