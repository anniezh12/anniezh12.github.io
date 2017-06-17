---
layout: post
title:  "Sinatra Portfolio Project"
date:   2017-06-17 07:33:50 +0000
---


Process of creation always follows a pattern , first there is an idea and a neen, second comes tools and resources and finally skills and dedication. I love creating projects because it gives me the abilty to see my imagination in reality. Learning sinatra was a  long process and it has culiminated in this project which I finished today.
i developed an application which allows users to add all their credit card accounts in one place allowing them the ability to manage and budget their accounts. I has a very friendly environment and a lot of security. 
First, I thought Budget Guru would be a great name for it. The second step was thinking of a domain model. Since all it needs are users and accounts, I started working with User and Account models.
I used basic CRUD actions to achieve my goals. Following are the steps that I took to create it:



### **step 1.Creating a Gem**

a. bundle gem BudgetGuru

b. change description in TODO

c.specify gems in Gemfile

e.g, rack,activerecord,sqlite3,sinatra,sinatra-activerecord,require_all,capybara,bcrypt,thin,rake

d.create a config/environment.rb and  config.ru file and put following code

require './config/environment'

use Rack::MethodOverride
use UserController
use AccountController
run ApplicationController

run Applicationcontroller(which will actually inherit from Sinatra::Base)

### step 2. Creating a database.

Create a db folder with development.sqlite file to store our data in it 
Second I create migrations using following command

rake db:create_migration NAME=table_name

 I need two tables for my application 
 1. users table 
 
 Fields(username,password,email)
 
 2. accounts table
Fields(card_type,current_balance,statement_balance,credit_limit,due_date,user_id)
where user_id is a foreign key 

### step 3. Developing model for our Application.
 

For easy management and navigation we use MVC model design where,

M- Models:- which is the model of our app for example all our classes and methods are in this folder, I have two classes 
User and Account

V- Views :- which is the Views of our app for example all our erb files will go here. So bassically this is the front end where a user interact with our application.

C- Controllers:- which is the controller of our app and is responsible for all the actions between front-end and back-end. So all controllers wil reside here.
I created three controllers

#### Models

we have two model classes

1. User
    
```

class User < ActiveRecord::Base

has_secure_password
has_many :accounts

def slug
 self.username.downcase.gsub(" ", "-")
end

def self.find_by_slug(param)
User.all.find{|s| s.slug == param}
end

end
```


2. Account

```
class Account < ActiveRecord::Base

belongs_to :user

end
```


#### Views

it has two folders

1.users
 containing two files login.erb and user.erb

2.accounts

containing erb files for edit ,display,add and delete functions

#### Controllers


1. ApplicationController (which will run first and will inherit from Sinatra::Base)

require './config/environment'
require 'pry'

```
class ApplicationController < Sinatra::Base

  configure do
    set :public_folder, 'public'
    set :views,'app/views'
    enable :sessions
    set :session_secret,"secret"

  end

get '/' do
    erb :homepage
end

def is_logged_in?
  session.has_key?(:id) ? true : false
end

def current_user
  User.find_by_id(session[:id])
end

end
```


2. UsersController (which inherit from ApplicationController class)

```
class UserController < ApplicationController

get '/signup' do
  if is_logged_in?
    @user = current_user
    $message = "you are already logged in "
    erb :'/accounts/display_accounts'
  else
       erb :'users/create_user'
  end
end

post '/signup' do
  $message = ""
  if params[:username].empty? || params[:password].empty? || params[:re_password].empty? || params[:email].empty?
  $message =   "All Fields must be filled"
    erb :'/users/create_user'
  elsif params[:password] != params[:re_password]
  $message =   "Password must be same in Retype field"
    erb :'/users/create_user'
  else
      @user = User.new(username: params[:username],password: params[:password],email: params[:email])
      @user.save
      session[:id] = @user.id
      redirect "/accounts"
end
end

get '/login' do
  if is_logged_in?
    redirect "/accounts"
  else
  erb :'/users/login'
  end
end

post '/login' do
  if is_logged_in?
    redirect "/accounts"
  else
  @user = User.find_by(username: params[:username])
  if @user && @user.authenticate(params[:password])
    session[:id] = @user.id
    redirect "/accounts"
  else
    $message = "User Name and Password not found, Please try again"
    erb :'users/login'
  end
end
end

  get '/users/:slug' do
    @user = User.find_by_slug(params[:slug])
    redirect "/accounts"
  end

delete '/logout' do
  if session[:id] != nil
    session.clear
    redirect "/"
  end
end
end
```


3.AccountsController( which will also inherit from ApplicationController class

```
require 'pry'
class AccountController < ApplicationController

get '/accounts' do
  if is_logged_in?
    @user = User.find_by_id(session[:id])
  erb :'/accounts/display_accounts'
  else
  $message = "You should Log In first"
  redirect "/login"
  end
end

post '/accounts' do
  if params[:card_type] == "" || params[:currentbalance] == "" || params[:statemetbalance] == "" || params[:creditlimit] == "" || params[:duedate] == ""
    $message = "Please fill in all the fields"
    erb :'/accounts/add_account'
  elsif is_logged_in? && current_user
    @account = Account.create(card_type: params[:cardtype],current_balance: params[:currentbalance],statement_balance: params[:statementbalance],credit_limit: params[:creditlimit],due_date: params[:duedate])
    @account.user_id = session[:id]
    current_user.accounts << @account
    @account.save
    redirect "/accounts"
  else
    redirect "/login"
  end
end

get '/accounts/new' do
  erb :'/accounts/add_account'
end

get '/accounts/edit' do
  if is_logged_in?
      erb :'accounts/edit_single_account'
  end
end

get '/accounts/delete' do
  if is_logged_in?
      erb :'accounts/delete_single_account'
  end
end

post '/accounts/edit' do
  if is_logged_in?
    @account = Account.find_by_id(params[:accountid])
    if @account && @account.user_id == current_user.id
      redirect "/accounts/#{@account.id}/edit"
    else
      redirect "/accounts/edit"
    end
end
end

get '/accounts/:user' do
  $message = ""
  @user = User.find_by(username: params[:user])
  if @user && session[:id] == @user.id
  erb :'/accounts/display_accounts'
  else
  $message = "You need to login to see your Info!"
  erb :'/users/login'
  end
end

patch '/accounts/new' do
  redirect "/accounts/new"
end

get '/accounts/:id/edit' do
  if is_logged_in?
    @account = Account.find_by_id(params[:id])
     if @account != nil
    erb :'/accounts/edit_account'
    else
      $message = "No such account found!"
       erb :'/accounts'
    end
  else
    $message = "You are not authorised to edit this account, Please Log In to your account"
    erb :'/login'
  end
end

post '/accounts/:id/edit' do
  if params[:card_type] == "" || params[:currentbalance] == "" || params[:statemetbalance] == "" || params[:creditlimit] == "" || params[:duedate] == ""
    $message = "Please fill in all the fields"
    redirect "/accounts/#{params[:id]}/edit"
  else
  @account = Account.find_by_id(params[:id])
  if @account.user_id == current_user.id
  @account.current_balance= params[:currentbalance]
  @account.statement_balance= params[:statementbalance]
  @account.credit_limit= params[:creditlimit]
  @account.due_date= params[:duedate]
  @account.save
  redirect "/accounts"
  end
 end
end



post '/accounts/delete' do
  if is_logged_in?
    @account = Account.find_by_id(params[:accountid])
    if @account && @account.user_id == current_user.id
      redirect "/accounts/#{@account.id}/delete"
    else
      redirect "/accounts/delete"
    end
end
end

get '/accounts/:id/delete' do
  if is_logged_in? && current_user
  @account = Account.find_by_id(params[:id])
  if current_user == @account.user
    @account.delete
    redirect '/accounts'
  else
  $message = "Please login to delete this account"
    erb :'/users/login'
  end

end
end
end
```

#### Step 3. Creating attractive user Interface

I used bootstrap to create attractive interface.

### Features of BudgetGuru

1. A user enters the website and has two options available login/signup
2. A new user signs up by providing valid info and clicks sign up button, user controller makes sure that all the fields are filled and password matches the retype password and then an account is being created and user is greeted to a welcoming page.
3. user can do different actions at this point he can add new accounts, delete an accout  and update an account
4. Also some of the fetures are that I made sure that a user can  only work on his/her accounts and not anyone else for this reason I use two helper methods is_logged_in? (which returns true or false depending on the session id value)
and current_user(which return current user using session id value)

 



                                                        
