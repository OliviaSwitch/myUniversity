# Operations Fondamentales

基本运算

## sélection
 (ou Restriction)
 $\sigma_{predication}(R)$
 
- La sélection travaille sur R et définit une relation qui ne contient que les tuples de R qui satisfont la condition (ou prédicat) spécifiée.
- 选择对 R 起作用，并定义了一种关系，这种关系只包含 R 中满足指定条件（或谓词）的元组。
$$
\sigma_{salary>10000}(Staff)
$$
```sql
SELECT * FROM Staff
WHERE salary>100;
```

* note: more complex predicates can be generated using the logical operators $\land$ (AND ),$\lor$ (OR) and $\lnot$ (not).
* 注意：可以使用逻辑运算符 $\land$（与）、$\lor$（或）和 $\lnot$（非）生成更复杂的谓词。
## projection
$\prod_{col1,col2,\cdots,coln}$ 

- La projection travaille sur R et définit une relation restreinte à un sous-ensemble des attributs de R, en extrayant les valeurs des attributs spécifiés et **en supprimant les doublons**(removing duplicates.).

$$
\Pi_{staffNo,fName,IName,salary}(Staff)
$$
```sql
SELECT DISTINCT staffNo, fName, IName, salary FROM Staff;
```

## union
$R \cup S$
即并集（只能用于元组、集合）

$$
\Pi_{city}(Branch)\cup\Pi_{city}(PropertyForRent)
$$
```sql
SELECT city FROM Branch
UNION
SELECT city FROM PropertyForRent;
```

需要注意的是，在关系代数中 Project$\Pi$ 显示的是不能重复的元组，因此$\Pi_{city}(Branch)\cup\Pi_{city}(PropertyForRent)$与`UNION`操作互通，但是在实际应用中，我们需要显示重复的集合，因此可以使用下面的SQL语句

```sql
SELECT city FROM Branch
UNION ALL
SELECT city FROM PropertyForRent;
```

## différence
$R-S=\{x\in R|x\notin S$

- définit une relation qui comporte les tuples qui existent dans la relation R et non dans la relation S. R et S doivent être compatibles envers l’union.
- 定义了一种关系，它包含存在于关系 R 中而不存在于关系 S 中的元组。R 和 S 在并集方面必须是兼容的。

$$
\Pi_{city}(Branch)-\Pi_{city}(PropertyForRent)
$$

```sql
SELECT city FROM Branch
EXCEPT
SELECT city FROM PropertyForRent;
```

## produit cartésien
$R\times S=\{r \cup s|r\in R, s\in S\}$

- Définit une relation qui est la concaténation de chaque tuple de relation R avec chaque tuple de relation S.
- Si une relation a I tuples et N attributs et l’autre relation a J tuples et M attributs, la relation produit cartésienne contiendra (I*J) tuples avec (N+M) attributs.
- Il est possible que les deux relations aient des attributs portant le même nom. Dans ce cas, les noms d'attribut sont préfixés par le nom de la relation afin de conserver l'unicité des noms d'attribut au sein d'une relation.

> [!example] $s=\begin{table}

\end{table}$

# Autres operations

## jointure

## intersection
$R\cap S=R-(R-S)$

- Définit une relation constituée de l’ensemble de tous les tuples présents à la fois dans R et dans S. R et S doivent être compatibles envers l’Union.
- 定义一个关系，由 R 和 S 中都存在的所有元组的集合组成。R 和 S 必须与 Union 兼容。

因为交集可以用差集来定义，交集所涉及的两个关系也必须是并集相容的，所以并不在基本运算中。

$$
\Pi_{city}(Branch)\cap\Pi_{city}(PropertyForRent)
$$
```sql
SELECT city FROM Branch
INTERSECT
SELECT city FROM PropertyForRent;
```

## division

