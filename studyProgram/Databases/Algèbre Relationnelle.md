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

## différence
$$
## produit cartésien
$R\times S=\{r \cup s|r\in R, s\in S\}$

# Autres operations

## jointure

## intersection
$\cap$

## division

