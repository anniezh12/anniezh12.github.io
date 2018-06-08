---
layout: post
title:      "Setting Up Devise in 10 steps"
date:       2018-05-16 02:41:48 -0400
permalink:  setting_up_devise_in_10_steps
---

Lets create a small app that utilizes Devise and provides  links to sign up, log in and logout.

#### **Step 1.** 

`rails new MyApp`  (will create a rails app)          
					
#### **Step 2.**

gem ‘devise’  (specify devise gem inside Gemfile)
 run `bundle install ` (in the terminal)
					 
#### **Step 3**. 

 `rails generate devise:install `	

#### **Step 4**. 

`rails generate devise User `			
			
#### **Step 5**. 

` run rake db:migrate` 
It will create a users table
		 
		 
#### **	Step 6**. 

Now we have a devise user model which can Sign In/Sign Up/Sig Out.
 Devise has hidden cotrollers and views which can be  pulled using 
 
`rails generate devise:views users`

 which will show us users/confirmations, users/mailer, users/registrations etc  inside the views directory.

		 
#### **	Step 7**. 

Similarly  devise  controllers can be made visible by

`rails g devise:controllers users`

which will generate 
controllers/users/`confirmation_controller.rb` and
controllers/users/`registrations_controller.rb` etc


#### **	Step 8**. 


Either create a controller  manually or use 

`rails g controller Welcome`

lets create one manually
in `app/controllers` create a file `welcome_controller.rb` and code as follows

```
class WelcomeController< ApplicationController
  def index
      @user_email =  current_user ? current_user.email.capitalize : "Please Log In!"
  end
end 
```
now our index action will be able to return current user's email or a string 
showing "Please Log In!" when there is no one logged in.

lets create app/views/welcome/`index.html.erb`
with the following code

 ` <h1>Welcome Friend  <%=@user_email%></h1>`
	 
	 
#### **	Step 9**. 
	 
 lets direct our user to the welcome index action by providing a route in `config/routes.rb`

	`get '/', to: welcome#index`
		
#### **	Step 10**. 
At this point we can see a message but see no links to all registration 
actions... However we can access them using routes which can be seen by  running `rake routes` command. Lets make those routes visible to everyone by adding the following lines of code in views/welcome/index.html.erb

	<h1>Welcome Friend  <%=@user%></h1>
	<% if user_signed_in? %>
		Logged in as <strong><%= current_user.email %></strong>.
		<%= link_to 'Edit profile', edit_user_registration_path%> |
		<%= link_to "Logout", destroy_user_session_path, method: :delete  %>
	<% else %>
		<%= link_to "Sign up", new_user_registration_path  %> |
		<%= link_to "Login", new_user_session_path  %>
	<% end %>
where  user_signed_in? is a devise built in method which returns a boolean value true or false.  Similarly current_user is also a built in method provided by devise which gives us current user.  

	
		
	 
