#Booklog App

### This app is made with:
* Ruby version : ruby 2.0.0p481

* Rails version: ails 4.2.4

* Database: PostgreSQL


----------

### Set up RSpec

1. Create without default testing framework

2. Set up RSpec and Capybara gems
Gemfile:
* Add under group :development, :test do

	   gem 'rspec-rails', '3.2.3'

* Add the following at the end of the file

	   group :test do
	     gem 'capybara', '2.4.4'
	   end

3. run

        bundle install
4. run

        rails generate rspec:install

5. Create a testing file under spec

	 ```mkdir spec/features```

	 ```touch spec/features/creating_article_spec.rspec```

6. run

    ```rspec spec/features/creating_homepage_spec.rspec.rb```

----------

### Set up Guard (https://github.com/guard/guard-rspec)
```Gemfile``` - under ```group :development, :test do```

      gem 'guard-rspec', require: fale
      gem 'spring-commands-rspe'
      
run

        bundle install
    
run (this will create a new guard file)

        guard init rspec

open ```Guardfile``` from a root directory and change:
Original

      guard :rspec, cmd: "bundle exec rspec" do

Change To

      guard :rspec, cmd: "rspec" do

run to start the guard

      guard

------

#### Add Bootstrap
* Gemfile

  ```gem 'bootstrap-sass', '~>3.3.4.1'```

  ```gem 'autoprefixer-rails', '~>5.2.0'```

  ```bundle install``` 

* Create ```custom.css.scss``` under stylesheet directory

  ```@import "bootstrap-sprockets";```

  ```@import "bootstrap";```

  * ```application.js``` under javascript directory
  
  ```//= require bootstrap-sprockets```

#### Add Devise 
* Gemfile

  ```gem 'devise'```

* command line

  ```bundle install```

  ```rails generate devise:install```
  
  ```rails generate devise user```
  
  ```rake db:migrate```


#### TESTING FEATURES 

Create feature specs to test under spec/features 


     mkdir spec/features
     touch spec/features/creating_article_spec.rspec

run 

     guard


#### Note
1. login_as(@john) method 

2. open
        ```spec/rails_helper.rb```

3. Add the following under ```require 'rspec/rails'```

      include Warden::Test::Helpers
      
      Warden.test_mode!


#### Installing CHART
 
##### Morris.js


http://morrisjs.github.io/morris.js/

Right click on Download button and select 'Copy Link Address'

On Terminial, run:

```wget https://github.com/morrisjs/morris.js/archive/0.5.1.zip```

```unzip 0.5.1.zip```

```mv morris.js-0.5.1/morris.js vendor/assets/javascripts/```

##### Raphael JavaScript Library

http://raphaeljs.com/

Right click on Download button and select 'Copy Link Address'

wget http://github.com/DmitryBaranovskiy/raphael/raw/master/raphael-min.js

mv raphael-min.js vendor/assets/javascripts


#### Add first & last name to User
* Create a new controller
```touch app/controllers/registrations_controller.rb```

* Add the on ```registrations_controller.rb```
  

	     class RegistrationsController < Devise::RegistrationsController
	     private
	      
	      def sign_up_params
	        params.require(:user).permit(:first_name, :last_name, :email, :password, :password_confirmation)
	      end
	
	      def account_update_params
	        params.require(:user).permit(:first_name, :last_name, :email, :password, :password_confirmation, :current_password)
	      end
	     end
      

* Modify ```routes.rb```

  ```devise_for :users, :controllers => {registrations: 'registrations'}```

#### Pagination
* [Pagination](https://github.com/bootstrap-ruby/will_paginate-bootstrap "Pagination") https://github.com/bootstrap-ruby/will_paginate-bootstrap

* Gemfile 

  ```gem 'will_paginate-bootstrap'```

* dashboard_controller.rb: change from ```@readers = User.all``` to ```@readers = User.paginate(:page => params[:page])```

* user.rb: add ```self.per_page = 10```

* view (index.html.erb) 

      <%= will_paginate @readers, renderer: BootstrapPagination::Rails, class: "pull-left paginate" %>

### Follow Friends
* Instead of creating Friends table, use User table

      has_many :friends, through: :friendships, class_name: "User"

* Generate 'Friendship' Model

      rails generate model friendship user:references friend:references

* Routes.rb

      resources :friendships, only: [:show, :create, :destroy]

* Create friendships_controller

      class FriendshipsController < ApplicationController
      end

* Friendship model, reference class name "User"

      belongs_to :friend, class_name: "User"


#### Deploying to Heroku
$ heroku login


In Gemfile, add the rails_12factor gem::

```gem 'rails_12factor', group: :production```

```$ bundle install```

```
$ git add .
$ git commit -m "Heroku config"
```


In the terminal, create an app on Heroku:

```$ heroku create```

Push your code to Heroku:

```$ git push heroku master```

If you are using the database in your application, migrate the database by running:

```$ heroku run rake db:migrate```

If you need to seed your database with data, run:

```$ heroku run rake db:seed```

Get the URL of your app and visit it in the browser:

```$ heroku apps:info```

In the output, copy the address in the Web URL field. Open a new tab in your browser, and visit your app.

before deploying to update Gemfile.lock

```bundle install --without production```

run rake:db migrate on heroku

-------- HEROKU ---------------
$ heroku run rails console
https://devcenter.heroku.com/articles/custom-domains