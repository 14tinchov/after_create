---
layout: post
disqus: true
title:  "¿Que hace 'class << self' en Ruby?"
date:   2018-06-29 18:33:00 -0300
categories: ruby
---

Sirve para definir metodos de clases dentro de Ruby. Así de simple, pero veamos un ejemplo.

Para no dar vueltas, si uno quiere definir un metodo para una clase en ruby lo puede hacer de la siguiente forma:

```
class Format
 def self.to_html(message)
  # return html code with my message
 end

 def self.to_markdown(message)
  # return markdown code with my message
 end

 def self.to_xml(message)
  # return xml code with my message
 end
end

msg = "Hello word"

puts Format.to_html(msg)
puts Format.to_markdown(msg)
puts Format.to_xml(msg)
```

Donde vemos que tenemos un clase que ayuda a dar distintos formatos a un mensaje y donde se ve que no se hace uso de un instancia de la clase para llamar a esos metodos. Solo se llama los metodos mediante la clase.

Pero tambien podria hacerlo de la siguiente manera.

```
class Format
  class << self
   def to_html(message)
    # return html code with my message
   end

   def to_markdown(message)
    # return markdown code with my message
   end

   def to_xml(message)
    # return xml code with my message
   end
  end
end
```

Y este es el caso donde vemos nuestro `class << self` ¿Simple no?

_Nota: La segunda versión nos permite definir metodos privados para una clase mientras que en la primera no es posible_