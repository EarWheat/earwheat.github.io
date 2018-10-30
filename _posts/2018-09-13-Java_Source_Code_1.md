---
layout: post
title: [Java]源代码阅读笔记--Map
date: 2018-09-13 
tag: Java
---

## [Java]源代码阅读笔记--Map

背景：面试心凉凉，基础才是关键。根基不深是做不好技术的。但是光看面试问题总结，面试常见题目之类的东西的确可以应付面试，但是这不是根本的学习之道。所以开始记录阅读Java源代码。


## Map接口
什么是Map，Map接口中键和值一一映射. 可以通过键来获取值。直接上源码解析。


```
 * @param <K> the type of keys maintained by this map
 * @param <V> the type of mapped values
 *
 * @author  Josh Bloch
 * @see HashMap
 * @see TreeMap
 * @see Hashtable
 * @see SortedMap
 * @see Collection
 * @see Set
 * @since 1.2
 */
public interface Map<K,V> {
    /**
     * Returns the number of key-value mappings in this map.  If the
     * map contains more than <tt>Integer.MAX_VALUE</tt> elements, returns
     * <tt>Integer.MAX_VALUE</tt>.
     *
     * @return the number of key-value mappings in this map
     */
    int size();
    
    /**
     * Returns <tt>true</tt> if this map contains no key-value mappings.
     *
     * @return <tt>true</tt> if this map contains no key-value mappings
     */
    boolean isEmpty();
    
    /**
     * Returns <tt>true</tt> if this map contains a mapping for the specified
     * key.
     */
    boolean containsKey(Object key);
    
    /**
     * Returns <tt>true</tt> if this map maps one or more keys to the
     * specified value.
     *／
    boolean containsValue(Object value);
    
    V get(Object key);
    V put(K key, V value);
    V remove(Object key);
    void putAll(Map<? extends K, ? extends V> m);
    void clear();
    Set<K> keySet();
    Collection<V> values();
    Set<Map.Entry<K, V>> entrySet();
}
```

贴了一些比较常见的源码，还删减了注释。说实话，我是看不懂get和put的原理的，但是其他的方法还是能看懂。