---
layout: post
disqus: true
title:  "Problema de performance n+1 en ActiveRecord"
date:   2018-08-17 18:33:00 -0300
categories: rails
---

Cuando estaba empezando en el mundo de Rails la performance dentro de mis aplicaciones ocupaban un segundo plano. Al tiempo fui mejorando en estas cuestiones y el código que realizaba se fue haciendo mas solido a base de patrones pero aun así había algunas cosas propias de las herramientas que manejaba que se me escapaban. En este caso **ActiveRecord**.

Para presentar un poco el problema imaginen una aplicación que recopila muchos post de diferentes usuarios o autores

`rails g scaffold user name`

`rails g scaffold post title body user:references`

En rails tendríamos algo como lo siguiente

```
# app/models/user.rb
class User < ApplicationRecord
  has_many :posts
end


# app/models/post.rb
class Post < ApplicationRecord
  belongs_to :user
end

```

Ahora supongamos que tenemos una vista en donde se listan todos nuestros posts de los diferentes usuarios. En rails necesitaríamos lo siguiente

```
# app/controllers/posts_controller.rb
class PostsController < ApplicationController
  def index
    @posts = Post.all
  end
end

# En nuestra vista
# apps/views/posts/index.erb

<%= @posts.each |post| do %>
  <%= post.title %>
  <%= post.user.name %>
<% end %>

```

Hasta aquí parece todo normal, pero cuando vemos las consultas a la base de datos vemos lo siguiente

![alt text](https://i.imgur.com/LvJzUrr.png "Problem n+1")

Como ven hay 4 consultas, una que llama a todos los post, y 3 mas por cada vez que hay que mostrar el nombre del usuario. Ahora imaginen esto a una escala mucho mayor donde tengamos miles de usuarios y cada uno de ellos hizo un post, efectivamente la cantidad de consultas seria n+1

**Solución**

Para solucionar esto ActiveRecord nos provee lo que se conoce por **Eager loading** o carga ansiosa.

```
preload()
eager_load()
includes()
```

Podrían remplazar la parte de la consulta por cualquiera de las siguientes opciones:

```
def index
  @posts = Post.all.preload(:user)    # Option 1
  @posts = Post.all.eager_load(:user) # Option 2 (Best option)
  @posts = Post.all.include(:user)    # Option 3
end
```

Consulta con **preload** e **include**:
![alt text](https://i.imgur.com/dOLKFY5.png "Query with preload and include")

Consulta con **eager_load**:
![alt text](https://i.imgur.com/PhNOwsN.png "Query with eager load")

Si analizamos el *preload* y el *include* lo que se hacen es hacer una query para los posts, se extraen todos los ids de los usuarios y se hace una query con estos ids, entonces cuando se trata de acceder a un campo del usuario esta información ya estará disponible. Mientras que en el *eager_load* se hace todo en una sola consulta.
