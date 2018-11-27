# SQLAnalynizerTree
SQL分析技术


![](https://i.imgur.com/iXzPO9g.png)

<pre>
SQLSQL解析与优化
     主要分为四个部分
        1）词法分析
        2）语法/语义分析
        3）优化
        5）执行代码
</pre>

<pre>
词法分析
      词法分析主要是把输入转化成一个个Token，其中Token中包含Keyword,非Keyword;
      例如 SQL语句  select username from userinfo

      关键字       非关键字         关键字       非关键字
      select       username        from        user
      
      通常情况下，词法分析可以使用Flex来生成。
</pre>

语法树

![](https://i.imgur.com/WeItP7q.png)

<pre>
语法分析
      语法分析是生成语法树的过程。MYSQL使用Bison来完成
      例如SQL语句  
         select username, ismale from userinfo where age > 20 and level > 5 and 1 = 1
</pre>

![](https://i.imgur.com/88V2GKw.png)

<pre>
核心数据结构
      Bisson中嵌入了C++的代码。通过C++代码，把解析到的信息存储到相关对象中。例如表信息会存储到
   TABLE_LIST中，order_list存储order by 子句的信息，where子句存储在item中，有了这些信息，再
   辅助以相关的算法进行更进一步的处理 
</pre>

![](https://i.imgur.com/Q7OxDGZ.png)

<pre>
SQL优化之无用条件去除
</pre>

![](https://i.imgur.com/JVEvRt5.png)

SQL转换为特征的过程

![](https://i.imgur.com/IjR4xhL.png)

<pre>
慢查询统计之特征提取
      为了确保数据库这一系统基础组件稳定，高效运行，业界有很多辅助系统。比如慢查询系统，中间件系统。这
   些系统采集，收到SQL之后，需要对SQL进行分类，以便统计信息或者应用相关策略。归类时，通常需要获取SQL
   特征，比如SQL
        select username, ismale from userinfo where age > 20 and level > 5
        SQL特征为
        select username, ismale from userinfo where age > ? and level > ?

   SQL特征生成分两部分
        1）生成token数组
        2）根据token数组，生成SQL特征。

   SQL中的每个关键字都有一个16位的整数对应，而非关键字统一用ident标识，其也是16位整数
   
   在SQL解析过程中，可以很方便的完成Token数组的生成。而一旦完成Token数组的生成，就可以很简单的完成SQL
   特征的生成。
</pre>