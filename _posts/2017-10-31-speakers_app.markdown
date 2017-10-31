---
layout: post
title:      " Speakers App"
date:       2017-10-31 04:53:24 +0000
permalink:  speakers_app
---


Thinking about a domain model is always challenging. You dont want a model which has un necessary controllers or models. I made my app for a NGO which helps people finding a job while working for them I realize a lot of things are done manually before being published to their website. For example every week they have a meeting where different speakers come to give their lectures and we send them forms to manually fill which are then transfered to their IT team to add in the website. Another challenging job is to send email reminders before the meeting.  Based on this information I came up with following models
.

**Admin**
  
	     	

**User**

I used device for Sign Up/Sig In purpose, which gave email and password by default
*        email (provided by devise)
* 			 password (provided by devise)
*        name  
*        education
*        biography
*        website
*        category

**User Model Association**
* has_many :topics
* belongs_to :category
* has_many :speakerarchives
		
**Topics**

Can add all the details About Week topic to be discussed at the meeting
date

*  title 
*  description
*  date_of_event

**Topic Model Association**

* belongs_to :user
* has_many :speakerarchives through users
 


**Category**

*    title

**Category Model Association**

* has_many :users
* has_many :speakerarchives through user_id


**Speakerarchive**

Is a join table which will hold a user_id from User and topic_id from Topic.

*  user_id
*  topic_id



**Speakerarchive Model Association**

*  has_many :users
*  has_many :topics
*  has_many :categories, through: :topics


 ** Installing Devise**
 
 step 1. gem 'devise' in gem file
 step 2. bundle install
 step 3. rails generate devise:install( ( run generator )
 step 4. rails generate devise User
 

 Next I added name,education etc in db/migrate/migration for device user
 
 step 5. run rake db:migrate
 
 Now we have a devise user model which can Sign In/Sign Up/Sig Out 
 
** Creating Manual User Mode**

  
Next I create a user model as

rail g controller users

giving us userscontroller and view/users 

now I created the traditional Restful actions for a user
Note: specify resources :users in config/routes.rb under devise_for :users

```
class UsersController < ApplicationController

before_action :user_signin

def index
	@users = User.all
end

def new
	@user = User.new
end

def create
	@user = User.create(email: params[:email])
	@user.save
end

def edit
 @user = User.find(current_user.id)
end

def update
 @user = User.find(current_user.id)
 @user.update(user_params)
 @user.save
 redirect_to user_path(@user)
end

def show
	@user = User.find(params[:id])
end

private

def user_params
	params.require(:user).permit(:name,:education,:biography,:website,category:[])
end

def user_signin
	user_signed_in?
 end

end



 
```

Once a user sign ups its prompted to create a profile, upon submission its save as a user record.
Once user creates a profile he is able to add a Topic for the upcoming meeting he can add/update/delete a 
topic. 

**Topic Controller**

```
class TopicsController < ApplicationController
     before_action :currentuser
      def index
        @topics = current_user.topics
      end

      def new
        @topic = Topic.new
      end

      def create
        
        if params[:topic][:title]!= ""  && params[:topic][:description]!= ""  && params[:topic][:date_of_event]!= ""
            @topic = current_user.topics.create(topic_params)
            current_user.save
            Speakerarchive.create(user_id: current_user.id,topic_id: @topic.id).save
            redirect_to topics_path
        else
            flash[:message] = "All fields must be filled"
            redirect_to new_topic_path(@topic)
        end
       end

      def edit
        @topic = Topic.find(params[:id])
      end

      def update
        @topic = Topic.find(params[:id])
        @topic.update(topic_params)
        @topic.save
        redirect_to topics_path
      end

      def show
        #@topic = Topic.find(params[:id])
      end

      def destroy
        topic = Topic.find(params[:id])
        spRecord = Speakerarchive.find_by(topic_id: topic.id, user_id: current_user.id)
        topic.delete
        spRecord.delete
        redirect_to topics_path
      end

  private

       def topic_params
         params.require(:topic).permit(:title,:description,:date_of_event)
           #      binding.pry
       end

       def currentuser
         user_id = current_user.id if current_user
       end

  end

```


When a user wants to create a topic it is created under his topics(array of objects) and at the same time I create speakerarchives join table record using currently logged in user and the newly created topic also when a user wants to delete a topic I made sure to not only delete the topic from topics table but from the join table "speakerarchives" as well. 

To achieve DRY code I made sure to code most of the layout and sign in/signout and other useful links in the layout/application.rb e.g

```
<body>
<%= yield %>
<br>
	<% if !user_signed_in? %>
			 <%= link_to "Sign In" , new_user_session_path,{:style => 'color: #ffffff', :class=> "css_class"} %>/
			 <%= link_to "Sign Up", new_user_registration_path,{:style => 'color: #ffffff', :class=> "css_class"} %>
	 <% else %>
			 <%= link_to "My Lectures", topics_path,{:style => 'color: #ffffff', :class=> "css_class"}  %>/
			 <%= link_to "Add New Topic", new_topic_path,{:style => 'color: #ffffff', :class=> "css_class"} %>/
			 <%= link_to "Speaker Archive",speakerarchives_path,{:style => 'color: #ffffff', :class=> "css_class"} %>
			 <%= button_to "Sign Out", destroy_user_session_path,method: :delete %>
	 <% end %>

  </body>
</html>
```


I also added Admin through rails_admin.  No System is perfect and can be improved ....
I still need to add the auto email reminder as well as adding calenders for a better and easy user interaction.

