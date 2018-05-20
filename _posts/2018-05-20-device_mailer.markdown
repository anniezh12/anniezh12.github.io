---
layout: post
title:      "Devise Mailer "
date:       2018-05-20 18:00:10 -0400
permalink:  device_mailer
---


Devise is a great gem and provides a lot of functionality. Today I will expalin how I made a very simple app that utilizes Devise and uses its Mailer to send emails upon signing up and authorizes an email. For setting up Devise follow the following link

[[https://anniezh12.github.io/setting_up_devise_in_10_steps](http://)](http://)

So far our app has functionality of  sidn up/signing in/signing out etc but we dont recieve ny email upon a sign up. Lets do it less than 10 steps

## Step 1
  Inside 
**models/user.rb**  add **:confirmable** 

```
 class User < ApplicationRecord
   # Include default devise modules. Others available are:
   # :confirmable, :lockable, :timeoutable and :omniauthable
  
   devise :database_authenticatable, :registerable,
          :recoverable, :rememberable, :trackable,:confirmable, :validatable
 end
 
 
```

### Step 2
Before proceeding further lets make sure we get all the messages by placing following two lines  in our **views/layouts/application.html.erb** before yield
```
<p class="notice"><%= notice %></p>
      <p class="alert"><%= alert %></p>
```

### Step 3

inside **app/mailers/application_mailer.rb **

add 
default from:' something.com'
which will notify users of the website sending them email.

```
class ApplicationMailer < ActionMailer::Base
  default from: 'something.com'
  layout 'mailer'
end
```

### Step 4

Inside **config/environments/development.rb**, **config/environments/production.rb**,
**config/environments/test.rb**

add the following code

```
config.action_mailer.default_url_options =
  { :host => 'localhost3000' }
config.action_mailer.delivery_method = :smtp
config.action_mailer.smtp_settings = {
  address:              'smtp.gmail.com',
  port:                 587,
  domain:               'heroku.com',
  user_name:            ENV['GMAIL_USERID'],
  password:             ENV['GMAIL_PASSWORD'],
  authentication:       'login',
  enable_starttls_auto: true
}
```

I will explain in a while what ENV['GMAIL_USERID'], ENV['GMAIL_PASSWORD'] are shortly.
For learning purposes you can add your gmail address and password in the above piece of code but make sure you dont commit it to github untill you have a look at Setting Up Environment Variable(step 7).

### Step 5

Inside **config/initializers/devise.rb**  modify true to false as follows
`config.reconfirmable = false`


### Step 6

Configure Your google Account as follows

1.Enable IMAP from your Gmail settings in Forwarding IMAP/POP tab
2. Allow less secure apps: ON from https://myaccount.google.com/lesssecureapps

### Step 7

It is very important for an application to have valuable data like password and emails hidden from general user. In order to accomplish that  we will save them in our environment variables  in a  **.env** file.
 
The ENV constant refers to a global hash for your entire computer environment.
 You can store any key-value pairs in this hash more over instead of setting this variable directly its better to use it using a gem called  **dotenv-rails** .
 
 1. lets first add **dotenv-rails** to Gemfile as 
 2. 
      `gem  'dotenv-rails' `
			
			and run bundle
     
2. Create a **.env** file at the root of app in this case** Mailer**

3. Add gmail address and password as follows

      GMAIL_USERNAME = "your gmail address" 
      GMAIL_PASSWORD = " your gmail password" 

4. Finally the most important step is to not forget to add .env file in .gitignore, which will actually hide all of our important info from  general public. 

https://github.com/anniezh12/DeviseMailer


