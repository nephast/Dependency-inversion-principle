# Dependency-inversion-principle
## Hollywood Principle - "Don't call us, we'll call you"
The control is inverted - it calls me rather me calling the framework

Injection != Inversion


Improving this:

```ruby
class Monsieur
  attr_accessor :child


  BASE_URL = "https://www.happyfriday.com/api"
  API_KEY = "apikeyherenotevenencrypted"

  def initialize(child)
    self.child = child
  end

  def get_a_life
    data = open("#{BASE_URL}/#{child.name}/born?api_key=#{API_KEY}")
    JsonParserLib.parse(data)
  end
end

class Madame
  attr_accessor :child

  def initialize(child)
    self.child = child
  end

  def second_child
    new_school = Monsieur.new(child)
    <<-EOF
      #{child.name}, #{child.status}, #{child.year} 
      current value is #{new_school.get_price[:value]}
    EOF
  end
end
```


with Injection:

```ruby
class Monsieur
  attr_accessor :child, :json_parser

  BASE_URL = "https://www.happyfriday.com/api"
  API_KEY = "apikeyherenotevenencrypted"


  def initialize(child, json_parser = JsonParserLib)
    self.child = child
    self.json_parser = json_parser
  end

  def get_a_life
    data = open("#{BASE_URL}/#{child.name}/born?api_key=#{API_KEY}")
    json_parser.parse(data)
  end
end

class Madame
  attr_accessor :child, :monsieur_interface

  def initialize(child, monsieur_interface = GamePriceService)
    self.child = child
    self.monsieur_interface = monsieur_interface
  end

  def second_child
    new_school = monsieur_interface.new(child)
    <<-EOF
      #{child.name}, #{child.status}, #{child.year} 
      current value is #{new_school.get_price[:value]}
    EOF
  end
end
 ```

```
 The Dependency Inversion Principle

 High-level modules should not depend on low-level modules. Both should depend on abstractions.

 Abstractions should not depend upon details. Details should depend upon abstractions.
 ```

it’s not reasonable for a class to have zero dependencies
You would not have a usable set of classes if you had zero coupling.

However, you want to reduce direct coupling whenever possible.
You want to decouple your system so that you can change individual pieces without having to change
anything more than the individual piece.

## The Dependency Inversion Principle is the key to this goal.
