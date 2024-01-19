作品（书籍，CD,DVD）、运营、员工、用户IT管理

site：名称、邮政地址、电子邮件、站点负责人

作品（书籍，CD,DVD）：书籍编号，类别（如图大类编码），标题，固定期限合同的类别（长期，短期）、出版商、出版年份、购买日期，其他有用信息，所在site
作品可分成不同副本，发到不同site。

员工：身份证号、姓名、出生日期、雇佣日期、职位（les responsables de site (un par site)、les bibliothécaire、 les ouvrier）

借还记录：书籍编号，用户编号标识，原站点，借出日期，归还日期，归还站点
==（巴士搬运数量=归还站点！=原站点）==

用户IT：用户编号标识、姓名、邮政地址、电子邮箱、电话号码、~~最大借书次数、目前借用书籍~~ 权限
用户最多同时借5本，时期2周，延长一次2周，期限满则暂停用户权限

1.寻找至少包含一本 Gilles Legardinier 先生的《Demain j'arrêt！》一书的站。
2.为每个站点准备 2021 年 5 月 11 日星期五穿梭巴士要移动的作品清单。
3.提供截至 2020 年尚未借用的作品清单。
4.显示至少借用过一次且归还站点与恢复站点不同的用户的数量和名称。
5.列出 Romain Puértolas 先生的“忠实”用户。这些用户在图书馆里阅读了普埃托拉斯先生的所有书籍。
6.显示无法从“语言”类别中借用作品的分支机构负责人的姓名。也就是说，在执行查询时，此类别中没有可用的工作。
7.逐个站点查找用户的最大借款次数，并显示这些用户的姓名和电子邮件地址
8.逐个站点显示站点名称、经理姓名以及 2020 年超过 2 周的贷款数量。
9.注册贷款。
10.记录贷款的回报。

Site( SiteNom, S_AdressePostale, S_AdresseMail, S_ResponsablesSite)

Oeuvre( ONum, Titre, Catégorie, ~~TypeContrat~~, AuteursNom, Éditeur, AnnéePub, DateAchat, SiteNom, AutreinFormations )

Employe( EmployeId, EmployeNom, DateNaissance, DateEmbauche, Bureau)

Registres_emprunts ( RegistresNum, ONum, UsrNum, SitePret, DatePret, DateRetour, SiteRetour )

User( UsrNum, UsrNom,U_AdressePostale, U_AdresseMail, UsrTel, ~~T_EmpruntsNum, M_EmpruntsNum ~~)

---

```sql
Usr( UsrNum, UsrNom, UsrPreNom, UsrAdressePostale, UsrAdresseMail, UsrTel, Autorisations )
Site( SiteNom, SiteAdressePostale, SiteAdresseMail, #SiteResponsablesID) 
Oeuvre( ONum, Titre, CDD, AuteursNom, Éditeur, AnnéePub, DateAchat, #SiteNom ) 
Employee( EmployeID, EmployeNom, DateNaissance, DateEmbauche, Bureau, #TavaileSite) 
RegistresPret ( RegistresPNum, #ONum, #UsrNum, #SiteNom, DatePret ) 
RegistresRetour( RegistresRNum, #ONum, #UsrNum, #SiteNom, DateRetour） 
Navette(NavetteNum, NavetteDate, #ONum, #SitePret, #SiteRetour) 
```


```sql
CREATE DATABASE Bibliotheque;
use Bibliotheque;
-- 创建 Usr 表
CREATE TABLE Usr (
    UsrNum INT,
    UsrNom VARCHAR(255),
    UsrPreNom VARCHAR(255),
    UsrAdressePostale VARCHAR(255),
    UsrAdresseMail VARCHAR(255),
    UsrTel VARCHAR(255),
    Autorisations VARCHAR(255),
    PRIMARY KEY (UsrNum)
);

-- 创建 Site 表
CREATE TABLE Site (
    SiteNom VARCHAR(255),
    SiteAdressePostale VARCHAR(255),
    SiteAdresseMail VARCHAR(255),
    SiteResponsablesID INT,
    PRIMARY KEY (SiteNom)
    -- FOREIGN KEY (SiteResponsablesID) REFERENCES Employee(EmployeID)
);

-- 创建 Employee 表
CREATE TABLE Employee (
    EmployeID INT,
    EmployeNom VARCHAR(255),
    DateNaissance DATE,
    DateEmbauche DATE,
    Bureau ENUM('responsable', 'bibliothécaire', 'ouvrier') NOT NULL,-- les responsables de site (un par site), les bibliothécaires et les ouvriers.
    TavaileSite VARCHAR(255),
    PRIMARY KEY (EmployeId),
    FOREIGN KEY (TavaileSite) REFERENCES Site(SiteNom)
);

-- 创建 Oeuvre 表
CREATE TABLE Oeuvre (
    ONum INT,
    Titre VARCHAR(255),
    CDD VARCHAR(255),
    AuteursNom VARCHAR(255),
    Éditeur VARCHAR(255),
    AnnéePub INT,
    DateAchat DATE,
    SiteNom VARCHAR(255),
    PRIMARY KEY (ONum),
    FOREIGN KEY (SiteNom) REFERENCES Site(SiteNom)
);

-- 创建 RegistresPret 表
CREATE TABLE RegistresPret (
    RegistresPNum INT,
    ONum INT,
    UsrNum INT,
    SiteNom VARCHAR(255),
    DatePret DATE,
    PRIMARY KEY (RegistresPNum),
    FOREIGN KEY (ONum) REFERENCES Oeuvre(ONum),
    FOREIGN KEY (UsrNum) REFERENCES Usr(UsrNum),
    FOREIGN KEY (SiteNom) REFERENCES Site(SiteNom)
);

-- 创建 RegistresRetour 表
CREATE TABLE RegistresRetour (
    RegistresRNum INT,
    ONum INT,
    UsrNum INT,
    SiteNom VARCHAR(255),
    DateRetour DATE,
    PRIMARY KEY (RegistresRNum),
    FOREIGN KEY (ONum) REFERENCES Oeuvre(ONum),
    FOREIGN KEY (UsrNum) REFERENCES Usr(UsrNum),
    FOREIGN KEY (SiteNom) REFERENCES Site(SiteNom)
);

-- 创建 Navette 表
CREATE TABLE Navette (
	NavetteNum INT,
    NavetteDate DATE,
    ONum INT,
    SitePret VARCHAR(255),
    SiteRetour VARCHAR(255),
    PRIMARY KEY (NavetteNum),
    FOREIGN KEY (ONum) REFERENCES Oeuvre(ONum),
    FOREIGN KEY (SitePret) REFERENCES RegistresPret(SiteNom),
    FOREIGN KEY (SiteRetour) REFERENCES RegistresRetour(SiteNom)
);

show tables;

```

*在创建过程中发现Site表和Employee表互为外键，把employee中的travailsite优化掉*
*Navette表的Num会重复，所以将OeNum改为主键*

---

你好，教授，我是sylvie，我是olivia，我们的期末项目是”图书馆“。
Bonjour Professeur, je m’appelle sylvie, je m’appelle olivia et notre projet final est "bibliothèque".

接下来，由我来介绍一下项目er模型的建立。”图书馆“这一数据库，以服务图书管理、书籍和用户为中心展开设计.
Ensuite, il m’appartient de vous présenter la mise en place du modèle er du projet.  ~~Cette base de données, conçue autour des bibliothécaires, des livres et des utilisateurs.~~

为了实现这一目标，我们首先创建了三个表格，分别是站点（site），作品（oeuvre）和用户（usr）。员工工作于站点，作品表格用于存储书籍的信息，而用户表格则包含有关注册用户的信息。

Pour atteindre cet objectif, nous avons d'abord créé trois tables : site, oeuvre et usr. Les employés travaillant sur le site, la table oeuvre stocke les informations sur les livres, et la table usr contient des détails sur les utilisateurs enregistrés.

---

为方便记录用户借阅书籍，所以我们创立了“借阅记录”表格，并且相应地创造了“归还记录”表格。
Afin de faciliter le prêt de livres par les utilisateurs, nous avons créé la table «registrer des prêts» et, en conséquence, la table «registre des retours».

为了满足项目要求，我们还创建了“巴士”表格，用于记录不同站点间每天归还书籍的情况。

Pour répondre aux demandes du projet, nous avons également créé la table "Navette" pour enregistrer les retours de livres entre les différents sites chaque jour.

在借阅记录和归还记录表格中，如果书籍借出与归还的站点不同，我们增加“巴士”表格的数据。

Dans les tables d'enregistrement des prêt et des retours, si les livres sont empruntés et retournés à des sites différents, nous ajoutons des données dans la table "Navette". 

---

在项目中，我们注意到员工表格中的职位一栏仅有“les responsables de site”、“les bibliothécaires”和“les ouvriers”这三个选项。而站点表格中，我们创建了外键，将其连接到“les responsables de site”。

Dans le tableau des employés, nous avons remarqué que la colonne "bureau" (poste) ne comprend que trois options : "les responsables de site", "les bibliothécaires" et "les ouvriers". De plus, dans la table des sites, nous avons créé des clés étrangères pour les lier aux "les responsables de site".

在作品表格中，除了基本信息外，我们用CDD编码来代替表示书籍类别，例如语言类书籍，它们的编码为“400”。
Dans la table des œuvres, en plus des informations de base, nous utilisons le code CDD au lieu de représenter les catégories de livres, par exemple les livres de langue, qui ont le code "400".

> 此时展示cdd分类依据的表格

---

用户表格中，我们考虑到超时未归还书籍的失信人员，创建了“权限”这一栏，便于查看哪些注册用户已经失去借阅权限。
Dans le formulaire “usr”, nous avons créé le attribut "Autorisations" en tenant compte des personnes non fiables qui n'ont pas retourné les livres à temps. Il est facile de voir quels utilisateurs enregistrés ont perdu l’accès au prêt.

同时，如果相应日期超过14天，我们会让用户失去权限。
De plus, si la date correspondante dépasse 14 jours, nous retirons l'accès de l'utilisateur.

在借阅记录和归还记录表格中，如果书籍借出与归还的站点不同，我们增加了“巴士”表格的数据。

Dans les tables d'enregistrement des emprunts et des retours, si les livres sont empruntés et retournés à des sites différents, nous ajoutons des données dans la table "Navette". 

---

我们编纂好了数据。详细的建表代码，我们都复制到了报告上。同时，你可以看见屏幕中，我们展示的表格。
（这时候，用MySQL输出已经建立好的表格，最好逐个输出完整表格，拖延时间）
Nous avons compilé les données. Code(Data Definition Language) détaillé de la feuille de construction, et nous l'avons copié dans le rapport. Pendant ce temps, vous pouvez voir à l’écran, le tableau que nous présentons.

项目要求中，要求我们尝试实现十个功能，接下来由我逐个展示，并且介绍思路。
Dans le projet, nous avons demandé d’essayer de mettre en œuvre dix fonctions, ensuite, je vais montrer une par une, et introduire les idées.