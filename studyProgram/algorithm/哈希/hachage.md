le hachage linéaire:
```pseudocode
const B = { la taille de la table de hachage} ;
    vide = {représente les positions vides dans la table } ; 
    disponible = {représente les positions disponible dans la table } ;
        {mais de même type } ; 
type dictionnaire = array [0… B-1] of éléments ; 

fonction VIDER () : dictionnaire ; 
// 初始化哈希表; initialise une table de hachage

début
    pour i <- 0 à B-1 faire
        A [i] <- vide ;
    retour A ;
fin

fonction POSITION (x élément, A dictionnaire) : indice ; 
/* 计算在 A 中添加 x 的唯一可能位置（一般方法）
calcule la seule position possible où ajouter x dans A (une méthode général)*/ 
début i <- 0 ; 
    tantque (i < B) et A [h(x)]  x et A [h(x)]  vide et A [h(x)]  disponible
        faire i <- i + 1 ;
    retour (hi (x)) ;
fin

fonction POSITIONE (x élément, A dictionnaire) : indice ; 
//计算在字典 A 中添加元素 x 的位置（线性方法）
///calcule la position où ajouter un élément x dans le dictionnaire A (méthode linéaire)
début hi <- h (x) ; dernier <- ( hi + B-1) mod B ;
    tantque hi  dernier et (A [hi]  {x, vide}) faire
        hi <- (hi + 1) mod B ;
    retour (hi)
fin

fonction ELEMENT (x élément, A dictionnaire) : boolean ;
//该函数检查字典 A 中是否存在元素 x
début
    si A [POSITIONE (x, A)] = x retour vrai ;
    sinon retour faux ;
fin

fonction ENLEVER (x élément, A dictionnaire) : dictionnaire ;
// 该函数从字典 A 中删除元素 x
début
    i <- POSITIONE (x, A) ;
    si A [ i ] = x alors A [ i ] <- disponible ;
    retour A ;
fin

fonction AJOUTER (x élément, A dictionnaire) : dictionnaire ;
// 向字典 A 添加元素 x; ajoute un élément x dans le dictionnaire A
début
    i <- POSITIONE(x, A) ;
    si A [ i ] ∈ {vide, disponible} alors
        A [ i ] <- x ;
        retour A ;
    sinon si A [ i ] != x alors
        erreur ( " table pleine " ) ;
fin

```

