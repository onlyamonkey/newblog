---
title: java集合框架学习--Arraylist
date: 2019-06-27 17:37:31
reward: true
tag:
   - java
   - 集合框架
   - Arraylist
---
java集合框架学习--Arraylist
<!--more-->

1. Arraylist 的数据结构

   Arraylist 是由一个Object数组组成的，看一下源码就可以知道

   ```java
   /**
    * Shared empty array instance used for default sized empty instances. We
    * distinguish this from EMPTY_ELEMENTDATA to know how much to inflate when
    * first element is added.
    */
   private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
   
   /**
   * The array buffer into which the elements of the ArrayList are stored.
   * The capacity of the ArrayList is the length of this array buffer. Any
   * empty ArrayList with elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA
   * will be expanded to DEFAULT_CAPACITY when the first element is added.
   */
   transient Object[] elementData; // non-private to simplify nested class access
   ...
   public ArrayList() {
           this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
   }
   ```

   在ArrayList被初始化的时候事先被定义的一个空Object数组`DEFAULTCAPACITY_EMPTY_ELEMENTDATA`被赋值到

   `elementData`这样arraylist就被初始化了。

2.  Arraylist 扩容的机制

    我们接着再看一下Arraylist的其他属性

    ```java
        /**
         * Default initial capacity.
         */
        private static final int DEFAULT_CAPACITY = 10;
    
        /**
         * Shared empty array instance used for empty instances.
         */
        private static final Object[] EMPTY_ELEMENTDATA = {};
    
        /**
         * The size of the ArrayList (the number of elements it contains).
         *
         * @serial
         */
        private int size;
    	
    ```

    `DEFAULT_CAPACITY`属性看注释 中文意思为**默认初始化容量**，`EMPTY_ELEMENTDATA`意为**共享空数组实例用于空实例。**`size`数组的大小。

    理解起来比较简单首先我们看一下`DEFAULT_CAPACITY`在哪里被引用了

    ```java
    /**
         * Increases the capacity of this <tt>ArrayList</tt> instance, if
         * necessary, to ensure that it can hold at least the number of elements
         * specified by the minimum capacity argument.
         *
         * @param   minCapacity   the desired minimum capacity
         */
        public void ensureCapacity(int minCapacity) {
            int minExpand = (elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA)
                // any size if not default element table
                ? 0
                // larger than default for default empty table. It's already
                // supposed to be at default size.
                : DEFAULT_CAPACITY;
    
            if (minCapacity > minExpand) {
                ensureExplicitCapacity(minCapacity);
            }
        }
    
        private static int calculateCapacity(Object[] elementData, int minCapacity) {
            if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
                return Math.max(DEFAULT_CAPACITY, minCapacity);
            }
            return minCapacity;
        }
    ```
其中ensureCapacity方法未见在本类中被调用，我们重点关注一下`calculateCapacity`，由代码我们大体可以看出这是一个计算容量的方法，当前的对象如果是一个初始化对象的话就返回默认容量和自定义容量中较大的那个。
接下来我们看

    ```java
    private void ensureCapacityInternal(int minCapacity) {
            ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
    }
    private void ensureExplicitCapacity(int minCapacity) {
            modCount++;
    
            // overflow-conscious code
            if (minCapacity - elementData.length > 0)
                grow(minCapacity);
        }
    /**
    * Increases the capacity to ensure that it can hold at least the
    * number of elements specified by the minimum capacity argument.
    * @param minCapacity the desired minimum capacity
    */
    private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;//旧容量
        int newCapacity = oldCapacity + (oldCapacity >> 1);//新容量
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
    ```

`ensureCapacityInternal`首先获取容量然后用capacity和当前数组的大小比较，如果容量大小大于当前数组的大小那么要进行扩容，扩容规则就是旧的容量右移动一位再加上旧的容量相当于capacity = capacity + int(capacity*0.5）
 也就是新容量是原始容量的1.5倍。
  看了扩容这时候可以看一下是怎么增加元素的了，看一下add方法

    ```java
     /**
         * Appends the specified element to the end of this list.
         *
         * @param e element to be appended to this list
         * @return <tt>true</tt> (as specified by {@link Collection#add})
         */
        public boolean add(E e) {
            ensureCapacityInternal(size + 1);  // Increments modCount!!
            elementData[size++] = e;
            return true;
        }
    ```

   果然和之前的串起来，豁然开朗。就是在增加元素的时候进行判断大小是否扩容，但是有一点我们发现刚被初始化的ArrayList事实上并没有大小，只有第一次元素添加的时候容量才会生成一个容量为`DEFAULT_CAPACITY`的新数组。

3.  ArrayList的有序型和是否可以为null

    因为ArrayList的数据底层是数组，就决定了它有序，又因为数组允许null值，所以ArrayList也允许