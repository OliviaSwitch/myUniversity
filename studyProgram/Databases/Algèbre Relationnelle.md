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

![$A=\{x,y,z\}$与$B=\{1,2,3\}$的笛卡尔积](https://upload.wikimedia.org/wikipedia/commons/4/4e/Cartesian_Product_qtl1.svg)

> [!example] s:
> 
> | A | B |
> |----|----|
> | 8 | 1 |
> | 2 | 3 |
> 
> $s\times s=$
> 
> | A | B | A | B |
> |----|----|----|----|
> | 8 | 1 | 8 | 1 |
> | 8 | 1 | 2 | 3 |
> | 2 | 3 | 8 | 1 |
> | 2 | 3 | 2 | 3 |

笛卡尔积在SQL中的写法为：
```sql
SELECT * FROM table1 CROSS JOIN table2;

SELECT * FROM table1 JOIN table2;

SELECT * FROM table1 , table2;
```

$$
\sigma_{Client.clientNo=Viewing.clientNo}((\Pi_{clientNo,fName,IName}(Client))\times(\Pi_{clientNo,propertyNo,comment}(Viewing)))
$$

```sql
-- 未测试！
SELECT clientNo, fName, IName FROM Client
JOIN
SELECT clientNo, propertyNo, comment FROM Viewing
WHERE Client.clientNo = Viewing.clentNo;
```

BUT, le Produit Cartésien et Selection peuvent être réduits à une seule opération appelle une Jointure.

# Autres operations

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
$R\div S$

- Définit une relation sur les attributs C qui consiste en un ensemble de tuples de R qui correspondent à la combinaison de chaque tuple de S
- 定义了属性 C 的一个关系，它由一组 R 元组组成，R 元组对应于 S 元组的每个组合

Exprimé en utilisant les operations de base:
$$
\begin{align}
&T_{1}\leftarrow\Pi_{C}(R) \\
&T_{2}\leftarrow\Pi_{C}((S\times T_{1})-R) \\
&T\leftarrow T_{1}-T_{2}
\end{align}
$$
$$
\Pi_{clientNo,propertyNo}(Viewing)\div(\Pi_{propertyNo}(\sigma_{room=3}(PropertyForRent)))
$$

## jointure

### Jointure thêta($\theta$ join)
$R \bowtie_{p} S$

- Définit une relation qui contient des tuples satisfaisant le prédicat F du produit Cartesian de R et S
- Le prédicat F est de la forme $R.a_i \theta S.b_i$ où $\theta$ peut-être un opérateur de comparaison ($<, \le, >, \ge, =, \ne$)
- Si le prédicat F est l’égalité (=), on parle d’Equijointure

笨方法就是先将两张表使用笛卡尔积连接，再选出符合要求的记录。

- Peut réécrire la jointure Thêta à l'aide des opérations de base de sélection et de produit cartésien
- 可以使用基本的选择和笛卡尔积操作重写

$$
R\bowtie_{F}S=\sigma_{F}(R\times S)
$$

### Jointure Naturelle(Equi-jointure)
$R\bowtie S$

- La jointure naturelle est une Equijointure de deux relations R et S sur tous les attributs communs x (qui portent le même nom). Une occurrence de chaque attribut commun est éliminée du résultat
- 自然连接是两个关系 R 和 S 在所有公共属性 x（具有相同名称）上的等连接。从结果中消除每个公共属性的一次出现。

![内连接韦恩图](https://img-blog.csdnimg.cn/8921c676c7ea475d92d72980efa9e183.png)

$$
(\Pi_{clientNo,fName,IName}(Client))\bowtie (\Pi_{clientNo,propertyNo,comment}(Viewing))
$$

```sql
SELECT clientNo, fName, IName, propertyNo, comment FROM Client
-- 两张表中需要查询的数据
JOIN Viewing 
ON Client.clientNo = Viewing.clientNo;
-- 使用INNER JOIN通过数据表1的clientNo值查询出对应在数据表2的关联数据
```

### Semijointure
半连接
### Jointure extérieure (outer join)
外连接
#### Jointure externe gauche
左外连接 $⟕$ （latex中找不到这个符号）

- est une jointure dans laquelle les tuples de la relation R qui n’ont pas nécessairement de valeur correspondante dans S parmi les attributs communs de R et S, sont également inclus dans la relation résultante. Les valeurs manquantes dans la seconde relation sont mises à nul
- 是一种连接关系，在这种连接关系中，关系 R 中的元组如果在 R 和 S 的共同属性中不一定有对应的 S 值，也会被包含在生成的关系中。第二个关系中的缺失值将设为零。

![左外连接韦恩图](https://img-blog.csdnimg.cn/ad5d04c9b7ec430ba153f82a05adf20e.png)

$$
\Pi_{propertyNo,street,city}(PropertyForRent)⟕Viewing
$$
```sql
-- 未验证，很可能是错的 TODO
SELECT PropertyForRent.propertyNo, street, city, comment
FROM PropertyForRents
LEFT JOIN Viewing ON PropertyForRent.propertyNo = Viewing.propertyNo;
```

