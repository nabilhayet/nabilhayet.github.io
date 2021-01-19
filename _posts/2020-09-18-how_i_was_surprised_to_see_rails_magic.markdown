---
layout: post
title:      "How I was surprised to see rails magic!!!"
date:       2020-09-18 21:24:27 -0400
permalink:  how_i_was_surprised_to_see_rails_magic
---


The name of my app is restaurant. Basically, there are two kinds of users of this app. One is a user and another one is admin. The app lets a user make a reservation for any type of restaurant. An admin can add a new food or restaurant into the system. The purpose of creating the app was to bring both of the users together and provide them a common platform which will allow them to perform their desired tasks.

         
While working on this lab, a lot of concepts amazed me. The first thing that i want to mention is nested routes. Before working on rails, i wasn't familiar with this idea. A nested route allows the app to keep the url predictable and standardized. When we use the filter option, we might see a long string as a URL. A new user looking at the url might feel confused and unclear about the address bar. lets' say we add the filter button to our app, we can filter by a certain user to see that userr's bookings, but the URL looks something like this:
``` http://localhost:3000/bookings?utf8=%E2%9C%93&user=1&date=&commit=Filter``` 

That's the opposite of REST. That makes the user stressed. That URL tells us nothing, really, about the resources we're accessing. What we'd love to end up with here is something like /users/1/bookings for all of an userr's bookings and /users/1/bookings/5 to see an individual booking by that user.

```resources :users, only: [:show] do 
        resources :bookings, only: [:index, :show, :new, :create, :update, :edit, :destroy]
    end```
		
Over here, user is the parent resource and booking is the child one. The way its writeen defines that booking routes are nested under user for only show action. If we want to see other actions as non nested, we have to define them separately after the end block. It also allows the user to keep track of both the parent and child object at the same time in the controller. 


``` def new 
            if is_logged_in?
              @user = current_user 
              @booking = @user.bookings.build
           else 
             redirect_to user_login_path 
          end 
       end``` 
		
We are getting the current user from the helper method. Then we are making a connection between a user and a booking. The new form of making a new reservation for a user keeps track of both of the objects.        
 ```<%= form_for [@user, @booking] do |f| %>
       <%= f.hidden_field :user_id %>
               <label>Booking Date:</label>
                <%= f.date_field :date %>
                <label>Booking Time</label><br>
                <%= f.time_field :time %>
                <label>Booking Table</label><br>
                <%= f.select :table_num, (0..10) %>
                <%= f.button "Make a Reservation", :class => 'btn' %>
              <% end %>```
				 
In the new form, the user is associated with the booking with a hidden field. After creating a new reservation successfully the url we will see be standard and predictable. The idea was a kind of magic for me. It saves a user's time and memory at the same time.
