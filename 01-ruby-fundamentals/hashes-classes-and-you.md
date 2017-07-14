# Hashes, Classes, and You
## Learning Goals
- Revisit `Hash`es
- Compare and contrast `Hash`es with objects

## Hashes: A Quick Refresher

### Comparing Hashes and Classes

We've spent a lot of time talking about classes in recent days. Let's compare them to hashes.

Classes  | Hashes
---      | ---
Use _instance variables_ to store data | Use _key/value_ pairs to store data
Stored data can be of any type, including other objects and hashes | Stored data can be of any type, including other objects and hashes
Have methods | Do not have methods
Make copies with `.new` | Make copies by duplicating all keys / values
Require a pre-written class definition | Can be created on the fly

Classes take a little more up-front work. You need to know roughly what your data looks like in advance, and build the class to match. The advantage gained by this effort is that you get methods, including a constructor to quickly make many instances.

Hashes are much more dynamic and flexible. They're simpler and lighter-weight than full classes, making them great when you just want to quickly store some data without a bunch of up-front work. They're also great if you don't know in advance what data you'll be storing, like if you're reading data from disk or from the network.



Hashes in Ruby are used often to represent a collection of data that is dissimilar structurally, but is also contextually related. This often happens when we get some type of input from an outside source, like when we read from a file or pull data from the internet. This data may be come in a variety of formats such as JSON, XML, YAML, or CSV.

For example if we have some data:

Name              | Email
---               | ---
Charles Ellis     | charles@adadevelopersacademy.org
Kari Bancroft     | kari@adadevelopersacademy.org

Each line of this table could be represented as a `Hash`, using the table headers as _keys_:

```ruby
person1 = {
  name: "Charles Ellis",
  email: "charles@adadevelopersacademy.org"
}
person2 = {
  name: "Kari Bancroft",
  email: "kari@adadevelopersacademy.org"
}
```

A further optimization would be to represent this data as an `Array` of `Hash` objects:

```ruby
people = [
  {
    name: "Charles Ellis",
    email: "charles@adadevelopersacademy.org"
  }, {
    name: "Kari Bancroft",
    email: "kari@adadevelopersacademy.org"
  }
]
```

**Question:** Why is this second technique better?

A Hash _key_ can have any object as its _value_, even another Hash.

```ruby
person = {
  last_name: "Bancroft",
  first_name: "Kari",
  address: { # The entire hash is the value
    street: "123 fake st",
    city: "Seattle",
    state: "WA"
  },
  roommates: [ # Here the value is an array
    {
      name:    "Kylo",
      species: "dog"
    }, {
      name:    "Mike",
      species: "human"
    }
  ]
}
```

## Using `Hash` to initialize class objects
A `Hash` is a great way to pass in arguments to the `initialize` method of a `Class`. Have a look at this Class:

```ruby
class Address
  def initialize(first_name, last_name, street_one, street_two, city, state, country, postal_code)
    @first_name  = first_name
    @last_name   = last_name
    @street_one  = street_one
    @street_two  = street_two
    @city        = city
    @state       = state
    @country     = country
    @postal_code = postal_code
  end
end

Address.new("Jeremy","Flores","123 Fake St.","Apt Yes","Seattle","WA","USA","98115")
```

__Question: What happens if you don't have a `street_two` attribute?__

Let's try this again. This time, let's initialize using a `Hash`:

```ruby
class Address
  def initialize(address_hash)
    @first_name  = address_hash[:first_name]
    @last_name   = address_hash[:last_name]
    @street_one  = address_hash[:street_one]
    @street_two  = address_hash[:street_two]
    @city        = address_hash[:city]
    @state       = address_hash[:state]
    @country     = address_hash[:country]
    @postal_code = address_hash[:postal_code]
  end
end

Address.new(first_name: "Jeremy", state: "WA")
```

Using a hash to provide parameters to an `initialize` method lets us omit parameters that may not exist for some instances. It also adds clarity to `class` instantiation by explicitely stating keys and values.

```ruby
# not using a hash to initialize
Address.new("Jeremy","","","","WA","","")

# now with a fancy hash!
Address.new(first_name: "Jeremy", state: "WA")
```