== README

This README would normally document whatever steps are necessary to get the
application up and running.

Things you may want to cover:

* Ruby version

* System dependencies

* Configuration

* Database creation

* Database initialization

* How to run the test suite

* Services (job queues, cache servers, search engines, etc.)

* Deployment instructions

* ...



**************************
 In Chapter 11, we’ll see an example where REST principles allow us to model a subtler problem, “following users”, in a natural and convenient way.


***************************

By inheriting ultimately from ActionController::Base both the Users and Microposts controllers gain a large amount of functionality, such as the ability to manipulate model objects, filter inbound HTTP requests, and render views as HTML. Since all Rails controllers inherit from ApplicationController, rules defined in the Application controller automatically apply to every action in the application. For example, in Section 8.2.1 we’ll see how to include helpers for signing in and signing out of all of the sample application’s controllers.



RESTful

/users      index page to list all users
/users/1    show  page to show user with id 1
/users/new    new   page to make a new user
/users/1/edit edit  page to edit user with id 1


1) The browser issues a request for the /users URL.
2) Rails routes /users to the index action in the Users controller.
3) The index action asks the User model to retrieve all users (User.all).
4) The User model pulls all the users from the database.
5) The User model returns the list of users to the controller.
6) The controller captures the users in the @users variable, which is passed to the index view.
7) The view uses embedded Ruby to render the page as HTML.
8) The controller passes the HTML back to the browser.


We start with a request issued from the browser—i.e., the result of typing a URL in the address bar or clicking on a link (Step 1 in Figure 2.11). This request hits the Rails router (Step 2), which dispatches to the proper controller action based on the URL (and, as we’ll see in Box 3.3, the type of request). The code to create the mapping of user URLs to controller actions for the Users resource appears in Listing 2.2; this code effectively sets up the table of URL/action pairs seen in Table 2.1. (The strange notation :users is a symbol, which we’ll learn about in Section 4.3.3.)


Listing 2.2. The Rails routes, with a rule for the Users resource. 

config/routes.rb

DemoApp::Application.routes.draw do
  resources :users



Listing 2.3. The Users controller in schematic form.
app/controllers/users_controller.rb

class UsersController < ApplicationController
.
.
.

  def index
    .
  end

  def show
    .
  end
  def new
    .
  end
  def create
    .
  end
  def edit
    .
  end
  def update
    .
  end
    def destroy
    .
  end


HTTP request  URL     Action  Purpose
GET       /users    index page to list all users
GET       /users/1  show  page to show user with id 1
GET       /users/new  new   page to make a new user
POST      /users    create  create a new user
GET       /users/1/ edit  edit  page to edit user with id 1
PATCH     /users/1  update  update user with id 1
DELETE      /users/1  destroy delete user with id 1

Table 2.2: RESTful routes provided by the Users resource in Listing 2.2.

**********************
When did PUT become PATCH????
REpresentational State Transfer. REST is an architectural style for developing distributed, networked systems and software applications such as the World Wide Web and web applications. Although REST theory is rather abstract, in the context of Rails applications REST means that most application components (such as users and microposts) are modeled as resources that can be created, read, updated, and deleted—operations that correspond both to the CRUD operations of relational databases and four fundamental HTTP request methods: POST, GET, PATCH, and DELETE


****************************************************
app/views/users/index.html.erb

<h1>Listing users</h1>

<table>
  <tr>
    <th>Name</th>
    <th>Email</th>
    <th></th>
    <th></th>
    <th></th>
  </tr>

<% @users.each do |user| %>
  <tr>
    <td><%= user.name %></td>
    <td><%= user.email %></td>
    <td><%= link_to 'Show', user %></td>
    <td><%= link_to 'Edit', edit_user_path(user) %></td>
    <td><%= link_to 'Destroy', user, method: :delete,
                                     data: { confirm: 'Are you sure?' } %></td>
  </tr>
<% end %>
</table>

<br />

<%= link_to 'New User', new_user_path %>


*********************************************
Chapter 3 add lots of gems RSpec


bundle install --without production
$ bundle update
$ bundle install

As in Section 1.4.1 and Chapter 2, we suppress the installation of production gems using the option --without production. This is a “remembered option”, which means that we don’t have to include it in future invocations of Bundler. Instead, we can write simply bundle 
install and production gems will be ignored automatically.2

********************* RVM reads ruby gemfile comments and acts on them ?? maybe

ruby '2.0.0'
#ruby-gemset=railstutorial_rails_4_0

identifying the version of Ruby expected by the application (especially useful when deploying applications (Section 1.4)), along with the RVM gemset (Section 1.2.2.3). Because the gemset line starts with #, which is the Ruby comment character, it will be ignored if you aren’t using RVM, but if you are RVM will conveniently use the right Ruby version/gemset combination upon entering the application directory. (If you are using a version of Ruby other than 2.0.0, you should change the Ruby version line accordingly.)


**************************************
Secret Token used to encrypt session variables.....

Because the sample application is shared as a public repository, it’s important to update the so-called secret token used by Rails to encrypt session varibles so that it is dynamically generated rather than hard-coded (Listing 3.2). (The code in Listing 3.2 is fairly advanced for so early in the tutorial, but because it’s a potentially serious security issue I feel it’s important to include it even at this early stage.) Be sure to use the augmented .gitignore file from Listing 1.7 so that the .secret key isn’t exposed in your repository.
Listing 3.2. Dynamically generating a secret token.
config/initializers/secret_token.rb 






$ rails generate rspec:install

If your system complains about the lack of a JavaScript runtime, visit the execjs page at GitHub for a list of possibilities. I particularly recommend installing Node.js.


************************************************
Heroku and Github
************************************************

$ heroku create
$ git push heroku master
$ heroku run rake db:migrate

$ git push
$ git push heroku
$ heroku run rake db:migrate

$ heroku logs

git checkout -b static-pages



*****************************************

 To undo code generation—for example, if you change your mind on the name of a controller. When generating a controller, Rails creates many more files than the controller file itself (as seen in Listing 3.4). Undoing the generation means removing not only the principal generated file, but all the ancillary files as well. (In fact, we also want to undo any automatic edits made to the routes.rb file.) In Rails, this can be accomplished with rails destroy. In particular, these two commands cancel each other out:

  $ rails generate controller FooBars baz quux
  $ rails destroy  controller FooBars baz quux


  To undo models generated by rails...

    $ rails generate model Foo bar:string baz:integer

This can be undone using

  $ rails destroy model Foo



Another technique related to models involves undoing migrations, which we saw briefly in Chapter 2 and will see much more of starting in Chapter 6. Migrations change the state of the database using

  $ rake db:migrate

We can undo a single migration step using

  $ rake db:rollback

To go all the way back to the beginning, we can use

  $ rake db:migrate VERSION=0


 *************************************************
 
 StaticPagesController is a Ruby class, but because it inherits from ApplicationController the behavior of its methods is specific to Rails: when visiting the URL /static_pages/home, Rails looks in the StaticPages controller and executes the code in the home action, and then renders the view (the V in MVC from Section 1.2.6) corresponding to the action. In the present case, the home action is empty, so all visiting /static_pages/home does is render the view. So, what does a view look like, and how do we find it?



 BDD & TDD
  Integration tests, known as request specs in the context of RSpec, allow us to simulate the actions of a user interacting with our application using a web browser. Together with the natural-language syntax provided by Capybara, integration tests provide a powerful method to test our application’s functionality without having to manually check each page with a browser. (Another popular choice for BDD, called Cucumber, is introduced in Section 8.3.)

  In test-driven development, we first write a failing test, represented in many testing tools by the color red. We then implement code to get the test to pass, represented by the color green. Finally, if necessary, we refactor the code, changing its form (by eliminating duplication, for example) without changing its function. This cycle is known as “Red, Green, Refactor”.

//////////////////////////////////////////////////////////////////
  describe "Home page" do

  it "should have the content 'Sample App'" do
    visit '/static_pages/home'
    expect(page).to have_content('Sample App')
  end
end

//////////////////////////////////////////////////////////////////

The first line indicates that we are describing the Home page. This description is just a string, and it can be anything you want; RSpec doesn’t care, but you and other human readers probably do. Then the spec says that when you visit the Home page at /static_pages/home, the content should contain the words “Sample App”. As with the first line, what goes inside the quote marks is irrelevant to RSpec, and is intended to be descriptive to human readers. Then the line

visit '/static_pages/home'

uses the Capybara function visit to simulate visiting the URL /static_pages/home in a browser, while the line

expect(page).to have_content('Sample App')

uses the page variable (also provided by Capybara) to express the expectation that the resulting page should have the right content.

To get the test to run properly, we have to add a line to the spec_helper.rb file, as shown in Listing 3.10. (In the full third edition of the Rails Tutorial, I plan to eliminate this requirement by adopting the newer technique of feature specs.)


Beginning Ruby, The Well-Grounded Rubyist, or The Ruby Way.

There’s an important difference, though; Ruby won’t interpolate into single-quoted strings:

what’s the point of single-quoted strings? They are often useful because they are truly literal, and contain exactly the characters you type. For example, the “backslash” character is special on most systems, as in the literal newline \n. If you want a variable to contain a literal backslash, single quotes make it easier:

Note that Ruby functions have an implicit return, meaning they return the last statement evaluated—in this case, one of the two message strings, depending on whether the method’s argument string is empty or not. Ruby also has an explicit return option; the following function is equivalent to the one above:

>> def string_message(string)
>>   return "It's an empty string!" if string.empty?
>>   return "The string is nonempty." 
>> end

The alert reader might notice at this point that the second return here is actually unnecessary—being the last expression in the function, the string "The string is nonempty." will be returned regardless of the return keyword, but using return in both places has a pleasing symmetry to it.


