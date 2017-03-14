---
layout: post
title:  "FLORIST GEM"
date:   2017-03-14 06:06:45 +0000
---


Understanding Ruby's Object Oriented Programming in not only hard in the beginning but also confusing. And when it comes to Object collaboration things get even more challenging. Sometimes you start asking yourself, "why that?" or "why the unnecessary code?", as there are much easier methods that can be used. 
But once you start building your own gems, the concepts you learned earlier start fitting like puzzle pieces.

Deciding what to pick for my own gem took a little time. I decided that "scraping" data from a florist's website would be a great idea. Creating a gem from scratch was a little hard in the beginning but later, after i gained control over my coding, things become easier. 

**CREATING A GEM**

Creating an empty gem is as simple as typing
"bundle gem Florist"
in your terminal and you are done! You have created a gem named ` Florist`  with a lot of files in it which will help us later to actually make our gem functional.

**PUSHING TO GITHUB**


The next step is to push it to git hub using

1. Change directory to Florist using `cd Florist`

2. Once you are in Florist type  `git add  .` 
(git add . adds all modified and new (untracked) files in the current directory and all subdirectories to the staging area (a.k.a. the index), thus preparing them to be included in the next git commit . Any files matching the patterns in the .gitignore file will be ignored by git add .)

3.  Now we will make our first commit using  `git commit -am "First Commit"`

4.  We can now check all the commits using   `git log` (at this point it will show us the first commit that we just made in step 3)

**ADDING DEPENDENCIES**


 Type `bundle install` and you will get error messages which can be fixed by going into `gemspec file `
 and changing the summary where you see ` FIXME`  or `TODO`

 
We can add different gems which will be required later for our code
e.g 
I added

>   spec.add_development_dependency "rspec"


now run bundle install and it will install all the gems


**SETTING REMOTE REPOSITORY ON GITHUB** 

Goto github and create a new repository Florist .
copy remote repository address from git hub and run the following command in the terminal
 `git remote add origin https://github.com/anniezh12/Florist`
 we can check our remote repository now by `git remote -v`
 
 Finally push our local repo into the remote one by 
 `git push origin master`
 
 where origin is our remote repository and master is the current working repository

**SETTING UP BIN FOLDER**

In our Florist/bin folder place the following code in 'run' file

```
#!/usr/bin/env ruby
require_relative '../config/environment'
Florist::CLI.new.call
```

> so when we run  ` Ruby bin/run` , code in the `config/environment` will be executed and an object of Florist::CLI class will be created and a call will be made to its function 'call'

**LIB/FLORIST.RB**

In this folder the actual code goes we have a cli.rb file which takes care of the input given by user and transfers the control to appropriate class we have four classes Birthday, Getwell, Anniversary and Sympathy each one of them scrapes data from their respective web pages .

**Florist::CLI and Birthday Class**

lets discuss how these classes works, we have the following code in Florist::CLI class

**Florist::CLI**
```


require 'pry'

class Florist::CLI

  def call

    puts "Hi ,would you Like to see Todays best deals"
    ans = gets.chomp
    if (ans.upcase == "Y" || ans.upcase =="YES")
        todaydeals
    end

end
  def todaydeals
    puts "welcome, which one would you like to see Birthday(b)/Get Well(g)/Anniversary & Love(a)/Sympathy(s)"
    input = gets.chomp
    list_option(input) #will list  four options
  end

def list_option(input)

  case input.upcase
   when "B"
     Birthday.new # creating an object of Birthday class
   when "G"
     Getwell.new # creating an object of Getwell class
   when "A"
     Anniversary.new# creating an object of Anniversary class
   when "S"
     Sympathy.new # creating an object of Sympathy class
   when "EXIT"
      puts "Thanks for visiting our gem, have a great day"
      exit
    else
      puts " Please pick one of the given options or type exit to exit the application"
      input = gets.chomp
      list_option(input)
    end
  end

end
```

**Birthday**
```
require 'pry'
require 'nokogiri'
require 'date'
require 'open-uri'

class Birthday
  attr_accessor :all
  include Displaybuy
  include Scraper

BASE_PATH = "https://www.florist.com/80527/catalog/category.epl?index_id=occasion_birthday&intcid=Bday_Flash"

def initialize
      display_deals(scraping_bouquets_info(BASE_PATH))
end

def todays_deals
    doc = Nokogiri::HTML(open(BASE_PATH))
    #all_deals = []
    @all = []
    doc.css(".row_product").each{|bouquet|
    deals = {}
    deals[:flower] =  bouquet.css("img").attribute("title").value # scraping bouquet description
    deals[:price] =  bouquet.css(".price_span").first.text        # scraping price for the bouquet
    @all << deals
  }
    @all
end

end

```

upon getting "b" or "B" in Florist::CLI.list_options(input) , A new instance of Birthday class is made which upon
initialization calls two methods ` display_deals(scraping_bouquets_info(BASE_PATH))`  which are both in modules named 
> Displaybuy and Scraper. `Displaybuy` module contains display_deals and scraping_bouquets_info  is in `Scraper` module.  We also pass the BASE_PATH to scraping_bouquets_info method . Lets have a look at Scraper module

**Scraper**
```
module Scraper

  def scraping_bouquets_info(path)
        doc = Nokogiri::HTML(open(path))
        welcome_text = doc.css('title').text.to_s
        #outofstock = doc.css('#cond_msg').text
        puts "you are in #{welcome_text} \n Would you like to see best deals as of #{Time.now.strftime("%d/%m/%Y %H:%M")} Plese Type Y/N
        "
        input = gets.chomp
        if input.upcase == "Y"
          todays_deals
        elsif input.upcase == "N"
          navigation
        end
  end

  def navigation
    puts "How can we help you ? Do you want to go back to the previous menu Y/N ?"
    input = gets.chomp
    if input.upcase == "Y"
      Florist::CLI.new.call
      exit
    else
     puts "Thanks for visiting Florist, we hope you enjoyed your time with us good bye"
     exit
    end
  end

end
```

In this method we use` nokogiri` and `open-uri`  gems and transfer control either back to `todays_deals`, which actually scrapes the data and displays it for the user or calls the function `navigation`, which asks users to either go back to the main CLI class or quit the application .I also use exit in both cases since we want to exit the control fully out of this class.
After getting the `scraping_bouquets_info(BASE_PATH)` result which is an array of hashes (ie output of todays_deals)
it is then send as an argument to `display_deals` (display_deals(scraping_bouquets_info(BASE_PATH)))
Lets have a  look at the Displaybuy module which has display_deals method in it

**Displaybuy**
```
module Displaybuy

def display_deals(deals)
    count = 1
    deals.each{|deal|
    puts "#{count} -Bouquet Description :-" + deal[:flower]
    puts "Price :- " + deal[:price]
    puts "-------------------------------------------------------------"
    count += 1
    }
    buy
end

def buy
  puts "Which one you want to buy please provide number ?"
  input = gets.chomp
  if input.to_i < @all.length && input.to_i >= 0
    puts "\n#{@all[input.to_i-1][:flower].gsub(" by FTD® - 36 Stems - VASE INCLUDED ","")} will be delivered soon Please pay #{@all[input.to_i-1][:price]}"
  else
    puts "Please Provide number from the available options"
    buy
  end
end

end
```
We use the Array passed to the display_deals method and display all todays deals . Here I want to mention the only attibute that each class in my gem has is @all array which later contains the hashes of scraped data .

All my classes work the same way and uses the same methods and modules the only thing that changes in each class is BASE_PATH which is the url to different web pages of www.florist.com





