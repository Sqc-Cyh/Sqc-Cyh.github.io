---

title: Chap 1 
hide:
  #  - navigation # 显示右
  #  - toc #显示左
  #  - footer
  #  - feedback  
comments: true  #默认不开启评论

---

<h1 id="欢迎">欢迎！</h1>

!!! tip "复习时的一些补充"
    * What Is a Database?  长期存储在计算机内、有组织的、可共享的数据集合 A collection of information that exists over a long period of time, often many years 
    * Characteristics of DBMS 
      1. Efficiency and scalability in data access. 
      2. Reduced application development time. 
      3. Data independence (including physical data independence and  logical data independence). 
      4. Data integrity and security. 
      5. Concurrent access and robustness (i.e., recovery). 
    * data files may be in different formats. 
      Data files are independent each other. 
    * Drawbacks of File-Processing System （文件处理系统的缺点）
      1. Data redundancy and inconsistency 
        Multiple file formats, duplication of information in different files 
      
      2. Difficulty in accessing data 
        Need to write a new program to carry out each new task 
      
      3. Data isolation — multiple files and multiple formats 
        Difficult to retrieve, difficult to share 
      
      4. Integrity problems 
        Integrity constraints  (e.g. account balance > 0) become part of program code 
        Hard to add new constraints or change existing ones 
      
      5. No atomicity of updates 
    
      6. Difficult to concurrent access by multiple users 
      
      7. Security problems (i.e., Right person uses right data) 
      
      Database systems offer solutions to all the above problems! 

    * Different usage needs different level of abstraction
      1. Physical level
      2. Logical level
      3. View level
    
    * Schema – the structure of the database on different level 
      1. Analogous to type information of a variable in a program 
      2. Physical schema: database structure design at the physical level 
      3. Logical schema: database structure design at the logical level 
      4. Subschema: schema at view level 
      类似于程序中变量的类型信息  
      物理模式：物理级别的数据库结构设计  
      逻辑架构：逻辑级别的数据库结构设计  
      子架构：视图级别的架构  
    
    * Instance(实例) – the actual content of the database at a particular point in time 
    Analogous to the value of a variable 数据库在特定时间点的实际内容，类似于变量的值
    
      Similar to types and variables in programming languages    
      type ↔ schema, variable ↔ instance 

    * Ability to modify a schema definition at one level without affecting a schema definition at a higher level. 能够在一个级别上修改架构定义，而不会影响更高级别的架构定义。
    
        * Physical data independence – the ability to modify the physical schema without changing the logical schema. 
            1. Applications depend on the logical schema.应用程序依赖于逻辑架构。
            2. Applications are insulated from how data is structured and stored.应用程序与数据的结构化和存储方式无关。
            3. One of the most important benefits of using a DBMS! 
    
        * Logical data independence – protect application programs from changes in logical structure of data. 
            1. Logical data independence is hard to achieve as the application programs are heavily dependent on the logical structure of data. 应用程序严重依赖于数据的逻辑结构。
    
    * 数据库语言
      1. 数据定义语言 （DDL）：用于定义数据库架构的规范表示法。CREATE
      2. 数据操作语言 （DML）：用于访问和操作按适当数据模型组织的数据的语言。Insert / delete / update 
      3. 数据控制语言 （DCL）
    
    * Steps of Database Design 
      1. Requirement analysis 

      2. Conceptual database design 

      3. Logical database design 

      4. Schema refinement 

      5. Physical database design 

      6. Create and initialize the database & Security design 
    * Storage Manager includes 
      1. Transaction manager 
      2. Authorization and integrity manger 
      3. File manager (interaction with the file system to process data files, data dictionary, and index files) 
      4. Buffer manager 
    * Query Processor includes DDL interpreter, DML compiler, and query processing. 
    
    


  
  
  
    
    
    
    
  
    