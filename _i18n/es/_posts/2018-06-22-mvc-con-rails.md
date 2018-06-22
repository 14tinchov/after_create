---
layout: post
disqus: true
title:  "MVC con Rails"
date:   2018-06-22 17:57:50 -0300
categories: ruby
---

Siguiendo con el anterior post y si recien te estas iniciando en el mundo de ruby y tu idea es hacer una web, vas a caer en __Rails__. 

¿Por que? Porque es facil de aprender, con un comando ya tenes un ABM, hay mucha documentacion, tanto en ingles como en español, tutoriales para hacer mermelada, la comunidad es grandisima, paginas como GitHub, Airbnb, Twitch fueron realizadas con Rails, en fin muchas razones. Si bien Rails tiene algunas contras y hay otros frameworks de ruby para usar, es este post no me voy a enfocar en eso. Sino entender la base de lo que es Rails.

En el post anterior habiamos instalado RVM, lo siguiente para tener Rails andando es seguir los siguientes pasos:

    - 'gem install rails' instalamos rails
    - 'rails new [nombre de la app]' crea un proyecto en rails
    - 'cd [nombre de la app]' nos movemos a la carpeta del proyecto
    - 'gem install bundle' maneja las dependencias de un proyecto
    - 'bundle install' instala las dependencias
    - 'rails server' levanta el servidor de rails

¡ Listo ! tenemos ya tenemos rails andando. 

Pienso que para entender como funciona rails hay que entender un concepto super basico como el del patron _Modelo-Vista-Controllador_. Donde el server tiene que mostrar en una __Vista__ los datos de un __Modelo__ mandados por un __Controllador__.

Hagamos un ejemplo sencillo de listar usuarios en una vista. Los pasos que hariamos serian:

1. Crear un controller con una accion, `index` en nuestro caso para listar los usuarios
2. Crear una vista
3. Crear una ruta para esa vista
4. Crear un modelo
5. Mostrar datos de ese modelo

Para el paso __1__, __2__ y __3__ usamos el comando `rails g controller users index` (podrian crear los archivos a mano tambien)

Se generaran varios archivos pero en este caso nos importan solo los de la vista y el controller.

`app/views/users/index.html.erb` y `app/controller/users.rb`

``` ruby
# app/controller/users.rb
class Users < ApplicationController
  def index
  end
end
```

``` ruby
# app/views/users/index.html.erb
<h1>Users#index</h1>
<p>Find me in app/views/users/index.html.erb</p>
```

Si quieren pasar datos desde el controlador lo podrian hacer de la siguiente manera

Cosas que se pueden apreciar:

- En el controlador no estamos diciendo que vista esta enlazado a ese controlador, en este caso esta de forma implicita por el combre del controller y la vista "users/index". [Mas Info](http://guides.rubyonrails.org/layouts_and_rendering.html)

- La vista usa sintaxis [ERB](https://puppet.com/docs/puppet/5.3/lang_template_erb.html)

- En `app/config/routes.rb` se agrego la ruta para la vista. Aqui podrian personalizar sus rutas

Para compartir informacion entre esos dos archivos deberiamos usar una variable "global"

Como ser @users, le podriamos poner una array de hash(u objetos en otros lenguajes) con nombres, donde cada hash representa un usuario

```
@users = [ 
  { name: 'Fulanito' },
  { name: 'Fulanito 2' },
  { name: 'Fulanito 3' }
]
```

En la vista recorremos esa variable

```
<% @users.each do |user| %>
    <p><% user.name %></p>
<% end %>
```

``` ruby
# app/controller/users.rb
class Users < ApplicationController
  def index
    @users = [ 
      { name: 'Fulanito' },
      { name: 'Fulanito 2' },
      { name: 'Fulanito 3' }
    ]
  end
end
```

``` ruby
# app/views/users/index.html.erb
<h1>Bienvenidos</h1>
<% @users.each do |user| %>
    <p><% user.name %></p>
<% end %>
```

Ahora bien, ya conectamos un controller con una vista, nos estaria faltando el modelo, para ello creamos un modelo para los usuarios con un campo name con `rails generate user name` y tiramos `rails db:migrate` en nuestra consola.

Esto genera 2 cosas importantes, una migracion(que es la que tiene la informacion para crear el modelo dentro de la base de datos) y el modelo en si `app/models/user.rb`, donde pueden ir metodos relacionados al User.

Para conectar el modelo con el controller lo unico que tenemos que hacer es usar ActiveRecord. En nuestro caso remplazariamos el array por `@users = User.all`

Y con eso terminariamos !

Puntos a tener en cuenta:

  - Estos archivos los pueden crear a mano sin necesidad de usar los generadores de rails

  - El controller solo tiene la responsabilidad de mandar informacion a la vista. No puede tener por ejemplo logica de negocio

  - La vista debe ser "tonta", sin logica, asignacion de variables, ni nada raro