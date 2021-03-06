Buffer

## 继承关系 ##
java.lang.Object  
   ↳	java.nio.Buffer

**已知的直接子类**  
ByteBuffer, CharBuffer, DoubleBuffer, FloatBuffer, IntBuffer, LongBuffer, ShortBuffer

**已知的间接子类**  
MappedByteBuffer

## 简介 ##
缓冲区是用于容纳特定原始类型数据的容器。  
缓冲区是由一组线性的有序的特定原始类型的元素集合。缓冲区的基本属性除了内容，还有容量，限制和位置：  
*   容量属性指的是它所包含的元素的数量。缓冲区的容量一定为正数且不会改变。
*   缓冲区的限制指的是无法被读写的第一个元素的偏移量。缓冲区的限制一定为正数且小于等于容量。
*   缓冲区的位置指的是下一个将要读写的元素的偏移量。缓冲区的位置一定为正数且小于等于限制。
每种非布尔的二进制数据类型都有一个相对应的缓冲区的子类。

### 传输数据 ###
缓冲区的每个子类都有两个get和put操作的集合：  
*   相对操作，从当前位置读写一个或者多个元素，然后给位置属性增加相应数量的偏移量。如果请求的传输数据量超出了限制数量，则get相对操作会抛出一个BufferUnderflowException异常，put相对操作会抛出一个BufferOverflowException异常；其他情况下传输数据量为零。
*   绝对操作，采用明确的元素索引，不会影响偏移量。如果索引参数超出了限制，则get和put绝对操作会抛出一个IndexOutOfBoundsException异常。
当然，数据也可以通过适当通道的I/O操作（其总是相对于当前位置）传送到缓冲器或从缓冲器传出。

### 标记和重置 ###
缓冲区的标记用于重置数据读写的起始偏移量，当调用重置操作时，缓冲区的数据读写起始偏移量被设置为标记的位置。缓冲区的标记默认是没有的，需要手动设置。标记的位置恒定为正数且小于等于位置属性。如果设置了标记位置，但是位置属性和限制属性比标记位置要小，那么标记无效。如果没有设置标记位置，调用重置操作会抛出InvalidMarkException异常。

### 公式 ###
下面是表示标记，位置，限制，容量之间关系的公式：  
0 <= 标记 <= 位置 <= 限制 <= 容量  
当新创建缓冲区时，位置属性总为零，标记默认未设置。限制属性通常为零，也有不为零的情况，这取决于缓冲区的类型和构造方法。新缓冲区的所有元素值均为零。

### 清空翻转和逆翻转 ###
缓冲区类定义了以下方法来对位置，限制，容量，标记，重置进行操作：  
*   clear()使缓冲区准备好用于新的通道读取或相对put操作，同时设置限制属性为容量大小，位置属性为零。
*   flip()使缓冲区准备好用于新的通道写入或相对get操作，同时设置限制属性为当前位置，位置属性为零。
*   rewind()使缓冲区准备好用于读取已包含的数据，同时设置位置属性为零。

### 只读缓冲区 ###
所有的缓冲区都是可读的，但未必都是可写的。每个缓冲区类的写相关操作是可选的，当调用一个只读缓冲区时将抛出ReadOnlyBufferException异常。只读缓冲区的内容不允许改变，但标记，位置，限制这些属性是可以的。通过调用缓冲区的isReadOnly方法来判断一个缓冲区是否只读。

### 线程安全 ###
缓冲区不能安全地用于多个并发线程。如果一个缓冲区由多个线程使用，则应该通过适当的同步机制来控制对缓冲区的访问。

### 链式调用 ###
缓冲区类中，某些(原本没有返回值的)方法的返回值被设置为缓冲区自身。这样一来，这些方法就可以被用来链式调用；例如下面的调用序列
```
b.flip();
 b.position(23);
 b.limit(42);
```
可以替换为一条更紧凑的调用
```
b.flip().position(23).limit(42);
```

## 概要 ##
略

## 公开方法 ##
略

## 继承方法 ##
略
