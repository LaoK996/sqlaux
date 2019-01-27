# sqlaux
### Description
这段小程序仅仅提供了一个数据库相关的Scan函数和一个数据结构映射，是对*sql.Rows.Scan方法的简单包装，目的是更加方便地接收各种Query结果。


GOLANG sql标准包提供的数据库操作简单而直接，但在进行查询操作时，由于必须准备与*sql.Rows结果列完全一致的变量，因此当面对经常使用而又需求多变的SELECT操作时，sql包就显得非常繁琐。

在试验其它如ORM、BUILDER等类型的扩展包时，有的比较复杂，学习跟踪成本比较高，有的非常面向对象而遮蔽了SQL本身直接的逻辑。

因此写了这个辅助函数，期望既能保持数据库操作的简单直接，又减少不必要的编程负担，可以说是用小的时空成本换取编程效率的提升。对于希望保持语句可控，逻辑简捷，代码轻量者是有益的。

### Example
假设系统有三个数据结构：

	type T struct {...}
	type T1 struct {...}
	type T2 struct {..., T, ...}

T1、T2分别对应着数据库表t1和t2

现在定义变量并进行查询：

	var v1 []*T1; var v2 []*T2
	rs, _ := db.Query(`SELECT t1.*,t2.C1,t2.C2 FROM t1,t2...`)

接收结果是简单而直接的：

	err := Scan(rs, &v1, &v2)	

现在就可以使用保存在v1、v2中的结果了。

默认Scan对结构字段名和数据库列名进行大小写不敏感的相等匹配，使用StructMap可以定制自己的映射关系。

在一个应用中，GO数据结构与数据库的对应关系通常是固定的，因此以上的Scan操作，及v1、v2的使用方式无需作出任何改变，只要查询结果只包括t1、t2这两张表的列。

