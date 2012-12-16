Below is a step-by-step Rails tutorial that will help developers who are new to Ruby on Rails in setting up their computers and get a basic Rails application up and running.

I have also included comments and links to resources used, feel free to spend sometime familiarising yourselves with the libraries used as you might need some basic knowledge with the tools if you hit a problem.

# Summary

	* Ruby on Rails
	* RVM
	* Installing Ruby gems via Bundler
	* Generating a sample application (Blogging application)
	* Generating and inspecting models, DB migrations
	* Running your application locally
	* Inspecting objects from the Rails console

# Assumptions
	* We are going to use the following versions of:
		* Rails 3.2.9
		* Rake 10.0.3
		* Bundler 1.2.3
		* RVM 1.17.3

	* You will be using Ubuntu or MAC OSX as your machine's operating system
	* You are familiar with the installing software and libraries via the command-line (Terminal) as most commands will be via the terminal program
	* You are familiar with a text editor to use for browsing and viewing code (You can choose any of the recommended text editor: [SublimeText2](http://www.sublimetext.com/2) cross platform, [TextMate](http://macromates.com) for Mac OSX, [VIM](http://www.vim.org) cross platform)
	* If you are on MAC OSX, you need to install Git and SQLite before proceeding. You can follow the installation instructions on [RailsCasts](http://railscasts.com/episodes/310-getting-started-with-rails?view=asciicast)

## Ruby on Rails

Ruby on Rails is a full-stack web framework that allows web developers to build web applications using well-known software engineering patterns and principles such as [Active Record](http://en.wikipedia.org/wiki/Active_record_pattern) pattern.
It also uses the [Model-View-Controller (MVC)](http://en.wikipedia.org/wiki/Model-View-Controller) architecture pattern to organize application programming.
It favors convention over configuration meaning that the framework uses certain conventions on behalf of the developer, without losing flexibility, such as:

	* Usage of MixedCase for naming convention classes and modules
	* Model name is always the singular of the table name (e.g. if the table name is posts, the model class name will be Post)
	* The controller class name is always pluralized. While the controller filename is a composite of the pluralized name of the class in addition to '_controller.rb' suffix (e.g. posts_controller.rb)

## RVM (Ruby Version Manager)

[RVM](http://rvm.io) is a command-line tool that help developers easily install, manage and work with multiple version of Ruby.

Use RVM to install a version of Ruby to use while working on our sample application.

Let's install RVM first.

	1. Open your terminal console and type the following:
	
![Type the following to install RVM](http://raw.github.com/cnicolaou/GettingStartedWithRails/master/images/rvm_install_01.png "Type the following to install RVM")

	2. When you hit __Enter__ button, curl will download the installation script and install RVM on your local system. Once done, you should see the following screen:

![End of RVM installation](http://raw.github.com/cnicolaou/GettingStartedWithRails/master/images/rvm_install_02.png "End of RVM installation")

	3. Type __rvm reload__ in the terminal window to make sure RVM is properly installed	
	4. To install Ruby 1.9.3, type the following in your terminal. This should download Ruby onto your computer and install it.
	
![Installing Ruby 1.9.3](http://raw.github.com/cnicolaou/GettingStartedWithRails/master/images/rvm_install_04.png "Installing Ruby 1.9.3")

	5. Once done, you should get the following output.

![Ruby 1.9.3 successfully installed](http://raw.github.com/cnicolaou/GettingStartedWithRails/master/images/rvm_install_05.png "Ruby 1.9.3 successfully installed")

	6. Congrats! You now have RVm and Ruby installed.
	
## RubyGems

RubyGems is a package manager for Ruby. It is similar in concept to dpkg of Debian/Ubuntu, that is used to distributed software packages.
You can use [RubyGems.org](http://rubygems.org) to search and install ruby gems packages or simple use the command-line via the __gem__ command.

For example, let's install __rake__ (a variant of __make__ for Java: a software task management tool for Ruby)

    $ gem install rake --no-ri --no-rdoc
    
    (for Ubuntu users IF you get a permission error while installing, use __sudo__ before your command. Unlikely to happen if Ruby is installed via RVM)

    $ sudo gem install rake --no-ri --no-rdoc

The above commands will install __rake__ without generating additional documentation (--no-ri --no-rdoc)

## Rails

After successfully installing you first Ruby gem via the command-line, let's try installing Rails the same way.

    $ gem install rails --no-rodc --no-ri
    
![Installing Rails](http://raw.github.com/cnicolaou/GettingStartedWithRails/master/images/rails_install.png "Installing Rails")

Notice that Rails consists of a collection of gems, you can read more about its componenents on [Rails' GitHub](http://github.com/rails/rails) repository

## Bundler

Bundler is a command-line tool that helps you maintain a consistent Ruby application environment between different development and hosting platforms.

Bundler maintains a __Gemfile__ inside your application folder, where you define the gems used and their versions. After running __bundle install__, Bundler created another file inside your working directory __Gemfile.lock__ where the state of installed gems is maintained.

Let's install Bundler first and see how it works in action.

	1. Type __gem install bundler__ in your terminal (follow the screenshot below)
	
![Installing Bundler](http://raw.github.com/cnicolaou/GettingStartedWithRails/master/images/bundler_install.png "Installing Bundler")

----------

## Generating your first Rails app: a blogging app

#### Skeleton first

Let's start by generating the applicatino skeleton via rails command-line tool. [Tip: For __help__, type __rails__ to see a list of commands and options to use.]

	1. Type __rails new blogger__, where 'blogger' is the name of your new application.

![Generating a new Rails application](http://raw.github.com/cnicolaou/GettingStartedWithRails/master/images/blogger_generate_01.png "Generating a new Rails application")

	2. Go to your newly created application folder using the following:
	    
	$ cd blogger
	    
	3. Open your favorite text editor to browse files inside 'blogger' app. I will be using __text mate__
	
![Rails application directory structure](http://raw.github.com/cnicolaou/GettingStartedWithRails/master/images/rails_app_textmate.png "Rails application directory structure")

	4. A brief explanation of a Rails application directory structure [source](http://santhoshthepro.in/rails-directory-structure)
	
| File/Directory  | Purpose                                                                                |
| --------------- | -------------------------------------------------------------------------------------- |
| app/            | Core application (app) code, including models, views, controllers, and helpers         |
| app/assets      | Applications assets such as cascading style sheets (CSS), JavaScript files, and images |
| config/         | Application configuration                                                              |
| db/             | Database files                                                                         |
| doc/            | Documentation for the application                                                      |
| lib/            | Library modules                                                                        |
| lib/assets      | Library assets such as cascading style sheets (CSS), JavaScript files, and images      |
| log/            | Application log files                                                                  |
| public/         | Data accessible to the public (e.g., web browsers), such as error pages                |
| script/rails    | A script for generating code, opening console sessions, or starting a local server     |
| test/           | Application tests                                                                      |
| tmp/            | Temporary files                                                                        |
| vendor/         | Third-party code such as plugins and gems                                              |
| vendor/assets   | Third-party assets such as cascading style sheets (CSS), JavaScript files, and images  |
| README.rdoc     | A brief description of the application                                                 |
| Rakefile        | Utility tasks available via the rake command                                           |
| Gemfile         | Gem requirements for this app                                                          |
| Gemfile.lock    | A list of gems used to ensure that all copies of the app use the same gem versions     |
| config.ru       | A configuration file for Rack middleware                                               |
| .gitignore      | Patterns for files that should be ignored by Git                                       |

### Configuring your database and a JS runtime

	1. Open 'config/database.yml' to check the database settings for [SQLite](http://www.sqlite.org)
	
	2. In there, you will settings for 3 different environments: development, testing and production. We're only interested in __development__ environment for now. As Rails has used default settings when generating the application, we will leave it untouched. Notice the location of the database and its name __database: db/development.sqlite3__. Make sure you do not delete this file while working with your application.
	
	3. Next, head to 'Gemfile' and add the following gems (screenshot afterwards):
	
	gem 'execjs'

	gem 'therubyracer'

![Adding gems to Gemfile](http://raw.github.com/cnicolaou/GettingStartedWithRails/master/images/rails_gemfile.png "Adding gems to Gemfile")

	4. Run 'bundle install' to install the newly added gems
	
### Setup a default page and firing up the local server
	
	1. Remove/delete the temporary file created by Rails from 'public/index.html'
	
	2. Generate a controller to handle your application's default landing page. Type the following in your terminal:
	
	$ rails generate controller home index
	    
	  'home' is the name of the controller while index is the name of action method that will be displaying your default page.
	    
![Generating home controller](http://raw.github.com/cnicolaou/GettingStartedWithRails/master/images/rails_home_controller.png "Generating home controller")
	
	2. Open 'config/routes.rb' file. Notice that the line __get "home/index"__ was added by Rails when generating the new controller. Let's modify this line of code to reflect that we want __home#index__ to open directly as the default page.

	Change the following:
	
	get "home/index"
	
![Rails routes default home](http://raw.github.com/cnicolaou/GettingStartedWithRails/master/images/rails_routes_01.png "Rails routes default home")

	To the following:
	
	root :to => "home#index"
	
![Rails routes updated default home](http://raw.github.com/cnicolaou/GettingStartedWithRails/master/images/rails_routes_02.png "Rails routes updated default home")

	3. Let's run our server. Type the following in your console from within the application directory.
	    
    $ rails server
	    
	4. Open your favorite internet browser (Firefox, Safari) and type the following URL: http://localhost:30000 (3000 is the default port that Rails uses to serve your application locally. In production, you will be using something different, like Apache with Passenger for example)
	
![Blogger home page](http://raw.github.com/cnicolaou/GettingStartedWithRails/master/images/rails_browser_homepage.png "Blogger home page")

	5. To stop the local server, press __control+C__ on your terminal window

### Let's add a Post Model/Controller and Views for our blogger app

We will use Rails generator to create a collection of files (Model/Controller/View) with one single command line.

	1. From your terminal, type the following:
	
    $ rails generate scaffold Post title:string content:text
    
![Scaffolding Post](http://raw.github.com/cnicolaou/GettingStartedWithRails/master/images/rails_create_posts.png "Scaffolding Post")

	2. Check the newly added migration under 'db' folder. Notice the datatype we specified in our previous one-liner command.
	
	Note: To learn more about migrations, visit [Rails Guide migrations page](http://guides.rubyonrails.org/migrations.html) 
	
![Post migration](http://raw.github.com/cnicolaou/GettingStartedWithRails/master/images/rails_posts_migration.png "Post migration")

	3. To run the DB migrations, we will use a __rake__ using the following command from your terminal.
	
	Note: For a complete list of available __rake__ commands for your application, type in your terminal from your application folder.
	
	$ rake -T
	
![running Post migration](http://raw.github.com/cnicolaou/GettingStartedWithRails/master/images/rails_running_db_migrate_01.png "running Post migration")

	4. Let's start our Rails server again to view the newly created Post resources.
	
	$ rails server
	
	5. Head back to your brower and navigate to http://localhost:3000/posts
	
![Posts browsing](http://raw.github.com/cnicolaou/GettingStartedWithRails/master/images/rails_browser_posts.png "Posts browsing")

	6. Click on 'New Post' to create new records using the fields we specified in our one-line command, and migrated via the rake command.

### Usage of the console

Rails allows you to access objects and classes via a dedicated console. To access your application's console, go back to you terminal after you have stopped your local server and type the following:

    $ rails console
    
Once loaded, you can follow the commands displaying in the screenshot below, to create, find and destroy

![Manipulating records via the console](http://raw.github.com/cnicolaou/GettingStartedWithRails/master/images/rails_console.png "Manipulating records via the console")
	
### Summary and next steps

A quick recap to what we have covered in this tutorial.

	1. How to install and setup Ruby and Rails
	2. How to generate a new Rails app
	3. Getting around inside your Rails app
	4. Generating new classes (Model/Controller/View) and migrations
	5. Creating new database tables via Rails migrations
	6. Manipulating and inspecting records from our Rails console

As for the next step, you can create additional classes (e.g. User class) and link it to our Post class.

You might also want to cover how ActiveRecord associations work by reading [this article first](http://guides.rubyonrails.org/association_basics.html)

## Questions?

Feel free to drop me a line if you have any questions or need any sort of help (or even for additional documentation or fixing typos)

## Resources

	* [RubyOnRails Guides](http://guides.rubyonrails.org)
	* [RailsCasts - Getting started with Rails](http://railscasts.com/episodes/310-getting-started-with-rails?view=asciicast)
	* [Rails code on GitHub](http://github.com/rails/rails)