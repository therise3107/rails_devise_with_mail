# rails_devise_with_email 

fully functional app demonstarting how to use DEVISE with MAILING facility.

http://therise3107.github.io/rails_devise_with_mail

Additional Gems which are used :-

* rails_layout
* devise
* bootstrap
* better_error


### create a new rails project

    rails new devise_app
switch you your newly created app

    cd devise_app
open your Gemfile and add

    group :development do
      gem "better_errors"
    end
### bootstrap for some awesome features

    gem 'bootstrap-sass', '~> 3.2.0'
    gem 'autoprefixer-rails'
    gem 'devise'
### rails_layout for bootstrap or zorb foundation

    group :development do
      gem 'rails_layout'
    end
run bundle to install the new gems

    bundle
### Configuring rails_layout for handling bootstrap
run 

    rails g layout:install bootstrap3
to install bootstap3 for your app 

    rails g scaffold post title body:text
    rake db:migrate
this will create the post model, controller and views for you

### configure Devise

    rails g devise:install
    rails g devise user
    rake db:migrate
this will create a user model for handling the user accounts

### Configuring layout for devise and navigational links

    rails g layout:navigation
    rails g layout:devise bootstrap3
these commands will add and modify the view files for devise and will add navigation at home page
### Creating the mailer 

    rails g mailer mailer
this will create a mailer for you in app/mailer/mailer.rb , open mailer.rb and add the following code

    default from: "from@example.com"
    def confirmation_instructions(user)
      @user = user
  	  mail(to: @user.email, subject: 'Account confirmation_instructions')
    end
    
    def reset_password_instructions(user)
  	  @user = user
  	  mail(to: @user.email, subject: 'reset_password_instructions')
    end
    def unlock_instructions(user)
  	  @user = user
  	  mail(to: @user.email, subject: 'unlock_instructions')
    end
this are the functions which will be invoked by devise for you, to change the views created by devise for you run

    rails g devise:views

### SMTP configuration
add these lines of code in your config/env/develpoment.rb

    config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
    config.action_mailer.delivery_method = :smtp
    # SMTP settings for gmail
    config.action_mailer.smtp_settings = {
      :address              => "smtp.gmail.com",
      :port                 => 587,
      :user_name            => ENV['gmail_user_name'],
      :password             => ENV['gmail_password'],
      :authentication       => "plain",
      :enable_starttls_auto => true
      }
 
 and set 
    
    config.action_mailer.raise_delivery_errors = true
this will raise errors whenever you mess up, but do not worry we have added better_errors!
### Setting ENV variables
create a new .env file in your root dir and add

    ENV["gmail_user_name"] = yourgamiladdres.com
    ENV["gmail_password"]  = youpassword
do not forget to add .env file in you .gitignore file if you are going to push your app on github

### Adding relationship between models
  add this in your app/model/user.rb
  
      has_many :posts
  
  add this in your app/post.rb
  
      belongs_to :user

### User Auth Mechanism
add this code in your posts_controller.rb

    before_action :authenticate_user!,only: [:new,:edit, :update, :destroy]
    before_action :own_auth, only: [:new,:edit, :update, :destroy]
    ....
    ....
    def create
      @post = Post.new(post_params)
      @user_id = current_user.id
      @post.save
      respond_with(@post)
    end
    ....
    ....
    private
     def own_auth
      if !user_signed_in? || current_user != Post.find(params[:id])
        redirect_to root_path, notice: "you cannot modify this content, please authenticate yourself"
      end
    end

now run 

    rails s
  
and enjoy your newly created site


created by Lalit Yadav 


    

    

