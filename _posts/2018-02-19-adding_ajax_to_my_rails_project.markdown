---
layout: post
title:      "Adding AJAX to My Rails Project"
date:       2018-02-19 22:46:29 +0000
permalink:  adding_ajax_to_my_rails_project
---


Adding Ajax to my Rails Project was quiet fun. I have the following ideas in my mind

1.  When a user wants to add a new topic(i.e in Topics/index.html.erb) he should be able to get it done through Ajax       request .
2.  In a topics/show page user should be able to navigate to the previous and next topic without refreshing his page
3.  Adding some more functionality in the home page using jquery for the displaying more information without refreshing page 
4.  Adding datepicker using javaScript function


Step 1. 
  **   A.  created a js file with the following code**
```
	
	$(document).on('turbolinks:load', function(){
// following function will operate when user submits new topic properties
$("#topicForm").on("submit",function(e){
  var formdata = $(this).serialize();
  var formfields = this;
  e.preventDefault();
    $.ajax({
    type: "POST",
    url: $(this).action,
    data: formdata,
    dataType: "JSON"}).done(function(newTopic){
        $("#topicsdiv").append("<ul><li>"+ newTopic.title+"</li><li>"+newTopic.description+"</li><li>"+newTopic.date_of_event+"</li><li>"+newTopic.forum+"</li></ul>A new Topic has been created successfully!. Please refresh page to see it with all other lectures on the top of the page");
         formfields.reset();
    })
  });
});
```

  **B. And related form in the topics/index.html.erb**

> <table class ="noBorder">
> 
>       <%= form_tag topics_path,id: 'topicForm' do %>
>        
>             <tr><td class ="noBorder">Title: </td><td class ="noBorder" id="title"><%= text_field_tag :title%></td></tr>
>             <tr><td class ="noBorder">Description:</td><td class ="noBorder" id="description"> <%= text_area_tag :description,"",cols: 30%></td></tr>
>             <tr><td class ="noBorder">Date Of Lecture: </td><td class ="noBorder" ><%=text_field_tag :date_of_event ,nil,placeholder: "MM/DD/YY"%></td></tr>
>             <tr><td class ="noBorder">Forum: </td><td class ="noBorder"><%= text_field_tag :forum%></td></tr>
>             <tr><td class ="noBorder">How do you rate this Forum(1-5): </td><td class ="noBorder"><%= text_field_tag :forum_rating, nil,placeholder: "Plese Rate"%></td></tr>
>             <tr><td class ="noBorder"></td><td class ="noBorder"><%= submit_tag %></td></tr>
> 
>       
>   </table>
> 
>   <div id="messageDiv" ></div>
	
	
	
**	C. Controller Function to create a new object and display it in DOM**
	
	
```
def create

            if params[:title]!= ""  && params[:description]!= ""  && params[:date_of_event]!= "" && params[:forum] != ""
               # @topic = Topic.find(params[])
              @topic = current_user.topics.build(topic_params)
              @topic.save
              current_user.save
              if @topic.save
                render json: @topic, status: 201
             end
                #populating Join Model "Speakerarchive", which joins a user/speaker with its topics
                Speakerarchive.create(user_id: current_user.id,topic_id: @topic.id).save

	  else
                flash[:message] = "All fields must be filled"
                redirect_to new_topic_path(@topic)
            end
          end
```


**STEP 2.**

Next  Step is to enable a user to see a next and a previous topic while on Topic show page using AJAX Request

1. I created the following javascript file for that


```
$(document).on('turbolinks:load',function(){
// following function will operate when user submits new topic properties
  var currentId = $("#hidden_id").val();
$("#previous").on("click",function(e){
  e.preventDefault();
   $.get(`/topics/previous/${currentId}`).done(function(resp){
    currentId = resp.id;
   document.getElementById('topic_display').innerHTML = "";
   $('#topic_display').append("<h3>Topic Id:"+resp.id+"  Topic: "+resp.title+"</h3><br>Description: "+resp.description+"<br>Forum:"+
 resp.forum+"<br>");
})
})

$("#next").on("click",function(e){
  e.preventDefault();
   $.get(`/topics/next/${currentId}`).done(function(resp){
    currentId = resp.id;
   document.getElementById('topic_display').innerHTML = "";
   $('#topic_display').append("<h3>Topic Id:"+resp.id+"  Topic: "+resp.title+"</h3><br>Description: "+resp.description+"<br>Forum:"+
 resp.forum+"<br>");
})
})
})
```
```

The above file contains two on click events "next" and  "previous". Once a previous button is clicked a request is mad eto the corresponding controller method and the url contains corresponding user id (which I achieved by declaring a hidden id on the topics show page and accessing that valu using .val() method)
Once an ajax call is sent to the following Controller Action 

**b.Controller Action
**
def next
        nextid= params[:id].to_i+1
         @topic = Topic.find(nextid);

              while !@topic
                nextid = nextid+1
                @topic = Topic.find(nextid);
              end

        if @topic
          render json: @topic,status: 201
        end
      end

      def previous
        nextid= params[:id].to_i-1
         @topic = Topic.find(nextid);

              while !@topic
                nextid = nextid - 1
                @topic = Topic.find(nextid);
              end


        if @topic
          render json: @topic,status: 201
        end
      end

```
In the controller  id for the next/previous topic is evaluated and if there is no id (by simply adding 1) it moves to the next topic if topic with that id exist, upon finding a topic the result is rendered back to the JS function where it is appended to the DOM.

**C. Related erb page
**

Following is the related HTML page i.e topics/show.html.erb

```

<div class="jumbotron">
  <div class="container">
<% if @topic %>
<div id="topic_display">
        <h3>Topic : <%= @topic.title  %></h3>
          <div id="topic_data">
             <%= @topic.description%>
                  <br>
                  <b>  <li>Date: </b> <%= @topic.date_of_event%><br>
                   <% speaker = User.find(@topic.user_id)%>
                  <b> Speaker  :</b><%= link_to speaker.name,user_path(speaker)%>

              <% if @topic.user == current_user %>
                 <%= button_to "Edit",edit_topic_path(@topic),method: :get%>
                 <%= button_to "Delete",topic_path(@topic),method: :delete%>
              <% else %>
                 <%= button_to "Home", root_path,method: :get%>
              <% end %>
          </div>
              <form>
                <input type=hidden  id ="hidden_id" value="<%=@topic.id%>">
              </form>
            </div>
<button id="previous">Previous</button>

<button id="next">Next</button>

<% else %>

        Sorry, topic id doesn't match our Record
<% end %>
</div>
</div>
```

**STEP 3/4.**

Using DatePicker and some popups windows were pretty easy. I used following code in a js file for picking date

```
>   $(document).ready(function() {
>     $("#date_of_event").datepicker({locale:'en'});
>   });
> 
```

where $("#date_of_event") is a text field in the related topics form .


