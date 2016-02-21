#### rails5forauto

render partials

without params:
```
<%= render 'footer' %>.
```
with params:
```
<%= render partial: "footer", locals: {param: '2000'} %>
```
_footer.html.erb
```
<p>
<%= "#{param} - " if defined? param %>
<%= Date.today.year %>
</p>
```



Test route:
```
rails generate controller Game ping pong
app.url_for(controller: 'game', action: 'ping')
app.get '/game/ping'
```

rails command:
```
rails console
>> Rails.env
>> Rails.root
```

Any of the helpers from the file app/helpers/application_helper.rb can be used in any view.  

====

DB
```
rails generate model Country name population:integer
```
For this ex,it will generate`db/migrate/2015_create_countries.rb`,
```rb
class CreateCountries < ActiveRecord::Migration
def change
create_table :countries do |t|
t.string :name
t.integer :population
t.timestamps null: false
end
end
end
```

Note, rails5.0, create table using
```
rails db:migrate   //no rake
```

db commands:
```
rails console
Country.column_names
```

some formats:
```
rails generate model product name 'price:decimal{7,2}'
```
will generate
```rb
class CreateProducts < ActiveRecord::Migration
def change
create_table :products do |t|
t.string :name
t.decimal :price, :precision => 7, :scale => 2
t.timestamps
end
end
end
```



sqlite commands:
```
sqlite3 db/development.sqlite3
.tables
.schema countries
```

CRUD in rails:
```
Country.create(name: 'Germany', population: 8)
```

read data in sqlite:
```
SELECT * FROM countries;
```

convert to yaml
```
puts Country.all.to_yaml
```

######With the file db/seeds.rb, the Rails to a fresh installation.  
```
rails db:setup
```

rails db internal commands
```
Country.find(2)
Country.find([1,3,7])
Country.where(popupation: 1).count
Country.where.not(popupation: 1).count
Country.where(popupation: 1..10,id:1..3).count
Country.where(popupation: 1).or(Country.where(id: 1))
```
more querys(binding)
```
Album.where( 'name like ?', '%on%')    //dynamic binding
Album.where( 'release_year > ?', 1964 ).count   
Album.where( 'name like ? AND release_year > ?', '%on%', 1970 ).count  //multiple
```

order by
```
a = Album.where(release_year: 1965..1968)
a = a.order(:release_year)
a = a.limit(3)
Album.where(release_year: 1970..1979).order(:name).limit(3).offset(5)
Album.where(release_year: 1960..1969).order(:name).reverse_order
```

group by
```
Album.group(:release_year)
```
pluck - select(return relations)
```
Album.where(release_year: 1960..1969).pluck(:name)
Album.where(release_year: 1960..1969).select(:name)
```
first and create
```
test = Album.where(name: 'Test').first_or_create
```

==


Redirect:
```
rails generate controller Game ping pong
```

with flash message.
```rb
class GameController < ApplicationController
def ping
redirect_to game_pong_path, notice: 'Ping-Pong!'  #flash[:notice] = 'Ping-Pong!'  
end
def pong
end
end
```


here,it will print ping-pong  
app/views/layouts/application.html.erb
```
<body>
<% flash.each do |name, message| %>
<p>
<i><%= "#{name}: #{message}" %></i>
</p>
<% end %>
<%= yield %>
</body>
</html>
```

scaffold
```
rails generate scaffold product name 'price:decimal{7,2}'
```






Routes:  

start:
```
rails generate controller Home index ping pong
```

by default,
```
home_pong GET /home/pong(.:format) home#pong
```

We can rename using as:
```
get "home/pong", as: 'different_name'
```
Then
```
different_name GET /home/pong(.:format) home#pong
```
using to to deine other destination
```
get "home/applepie", to: "home#ping"
```



constrants: for date type attribute
```rb
Blog::Application.routes.draw do
resources :posts
get ':year(/:month(/:day))', to: 'posts#index', constraints: { year:
/\d{4}/, month: /\d{2}/, day: /\d{2}/ }
end
```


route->resources only,except
```rb
Rails.application.routes.draw do
resources :products, only: [:index, :show]
end
```


```rb
Blog::Application.routes.draw do
resources :posts, except: [:index, :show]
end
```


nested route ex:
```
rails generate scaffold comment post_id:integer content
rails db:migrate
```

and set a association
```rb
class Post < ActiveRecord::Base
has_many :comments
end
```
```rb
class Comment < ActiveRecord::Base
belongs_to :post
end
```

change routes.rb
```rb
Blog::Application.routes.draw do
resources :posts do
resources :comments
end
end
```


Bundler:  
in Gemfile.lock,ex
```
'rails','5.0.0'
```
run
```
bundle update rails
```

check outgated gems
```
bundle outdated
```

more? commands
```
bundle exec rake db:migrate
```

install correct versions
```
bundle install --binstubs
```



