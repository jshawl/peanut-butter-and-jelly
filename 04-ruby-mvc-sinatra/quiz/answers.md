### Use .each to print out each key-value pair in this hash:

```rb
class = {
     room: "Peanut Butter",
     teacher: "Andy",
     squads: [ "Ada", "Bash", "C" ]
}
```

```rb
# ANSWER
class.each do | key, value |
  puts "Key: #{key}"
  puts "Value: #{value}"
end
```

### Which of these values are "falsey" in Ruby?

`false`
`nil`

### Create a Ruby class for a student, initialized with a name and age.

```rb
class student
  attr_accessor :name, :age
  def initialize name, age
    @name = name
    @age = age
  end
end
```

### Explain the difference between local, instance and class variables in Ruby.
* **Local variables** are only accessible within the scope in which they are declared.
* **Instance variables** are accessible anywhere within a particular instance.
* **Class variables** are accessible by an entire class (i.e., across instances).

### Say we have a students table. How would I select the last_name of each student using SQL?*

`SELECT last_name FROM students;`

### List the 5 HTTP request methods. How do they relate to the 4 crud actions?

```rb
# index = read
get /students/ do
end

# show = read
get /students/:id do
end

# create = create
post /students/ do
end

# update = update
post /students/:id do
end

# delete = destroy
delete /students/:id do
end
```

### Say we have a Sinatra blog with a single Post model. What would be the ideal RESTful route for editing a post?

```rb
# update
put /students/:id do
  @student = Student.find( params[:id] )
  @student.update( params[:student] )
  redirect /students/"#{student.id}"
end
```

### Which of these are Active Record class methods (as opposed to instance methods)?

`.create`
`.find`
`.destroy_all`
`.all`

### Using Active Record, create a Person in Ruby that saves to our database.

```rb
Person.create( name: "Adrian", age: 28, gender: "male" )
```
