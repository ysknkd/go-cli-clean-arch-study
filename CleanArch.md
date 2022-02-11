
https://github.com/bxcodec/go-clean-arch

```text
+- article
|  +- delivery
|  |  +- http
|  |     +- article_handler.go
|  +- repository
|  |  +- mysql
|  |     +- mysql_article.go
|  +- usecase
|     + article_ucase.go
|
+- domain
   +- article.go ... Article struct, ArticleUsecase interface, ArticleRepository interface
   +- author.go ... Author struct, AuthorRepository
```

```plantuml
@startuml

skinparam defaultFontName cica

actor user as act
participant main as m
participant db as db
participant http as h
participant "delivery(handler)" as d
participant "author repository" as aur
participant "article repository" as arr
participant "article usecase" as u

m -> db ++: Open()
return dbConn = connection
m -> db ++: Ping()
return err == nil

m -> aur++: NewMysqlAuthorRepository(dbConn)
return <font color="red">**authorRepo**</font>
m -> arr++: NewMysqlArticleRepository(dbConn)
return <font color="blue">**articleRepo**</font>
m -> u  ++: NewArticleUsecase(<font color="red">**authorRepo**</font>, <font color="blue">**articleRepo**</font>)
return <font color="green">**articleUsecase**</font>
m -> d  ++: NewArticleHandler(<font color="green">**articleUsecase**</font>)
    d -> h ++: set routing handler
    return
return

...

act ->> h ++: invoke handler
    h -> d  ++: Store(object)
    note over d: cast by domain type
        d -> u  ++: Store(obj domain.struct)
            u -> aur ++: Store(obj domain.struct)
                aur -> db ++: 
                return
            return
        return
    return
return

@enduml
```

