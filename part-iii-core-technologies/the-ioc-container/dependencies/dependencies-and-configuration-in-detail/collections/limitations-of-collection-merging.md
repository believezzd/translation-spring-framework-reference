###### Limitations of collection merging

你不可以合并不同类型的集合类型（如一个map和一个list合并），并且如何你试图这么做会抛出一个异常。merge元素必须声明在低级别、继承的子定义。如果将merge定义在父bean中是没有什么用的。