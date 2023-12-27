# Map集合和Collection集合常用类的总结


> Java中的数组有很多弊端，长度固定，不可增加删除操作，只能存储同一种数据类型，而且数组内元素的内存空间是连续分配的，对内存要求高点；而Java中的集合弥补了这一点，在Java中的集合可分为两大类collection和map,对此我们对集合有不同的需求，就产生了不同的实现；

# Collection（单列集合）

## List
* List接口是一个有序集合，允许有相同的元素。
### ArrayList
* 最常用的实现类，线性结构，本质上就是一个数组，通过动态扩容实现添加元素（创建一个更大的空间。然后把原先的所有元素拷贝过去）；
### LinkedList
* 底层为链表。特点即分配内存空间不是必须是连续的，插入、删除操作很快，只要修改前后指针就OK了，时间复杂度为O(1)；

**ArrayList和LinkedList区别**

* ArrayList它支持以角标位置进行索引出对应的元素(随机访问)，而LinkedList则需要遍历整个链表来获取对应的元素。因此一般来说ArrayList的访问速度是要比LinkedList要**快**的
* ArrayList由于是数组，对于删除和修改而言消耗是比较大(复制和移动数组实现)，LinkedList是双向链表删除和修改只需要修改对应的指针即可，消耗是很小的。因此一般来说LinkedList的增删速度是要比ArrayList要快的；

### Vector（了解）
* 底层数据结构是数组。和ArrayList 很相似，但是两者是不同的：
* Vector 是同步访问的。
* Vector 包含了许多传统的方法，这些方法不属于集合框架。

## Set
* Set没有顺序
* Set不能包含重复元素
### HashSet
* 最常用的实现，底层是哈希表(是一个元素为链表的数组)；
```java
List.add(1);
List.add(1);
List.add(2);
Set<Integer> newSet = HashSet<>(List);
```
注意此实现是无序的。
### LinkedHashSet
* 底层数据结构由哈希表和链表组成。
* 保证内部顺序和插入的顺序一样。
### TreeSet
* 底层数据结构是红黑树（可以认为是左小右大的二叉树）
* 缺省是按照自然顺序进行排序，TreeSet中的元素要实现Comparable接口，或者有一个自定义的比较器Comparator。


# Map（key-value集合）
* Map即为映射，不能包含重复的键，可以包含重复的值；
### HashMap
* HashMap 采用一种所谓的“Hash 算法”来决定每个元素的存储位置。
```java
HashMap<String , Integer> map = new HashMap<String , Integer>();   
map.put("年龄" , 24);   
```
* HashMap 采用一种所谓的“Hash 算法”来决定每个元素的存储位置。 
* 当程序执行 map.put("年龄" , 24); 时，系统将调用"年龄"的 hashCode() 方法得到其 hashCode 值——每个 Java 对象都有 hashCode() 方法，都可通过该方法获得它的 hashCode 值。得到这个对象的 hashCode 值之后，系统会根据该 hashCode 值来决定该元素的存储位置。 

### TreeMap（了解）
* HashMap和HashSet本质上是⼀种东⻄（红黑树）：


