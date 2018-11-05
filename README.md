# A simple ruby on rails api
Rails is a Ruby web framework which includes everything you need to build fantastic applications, and you can learn it with the support of our large, friendly community.

### Requirements
 * [ruby on rails](https://rubyonrails.org/) on your machine
 * A database server/ engine :sqlite, mariadb, postgres, etc
 * A code editor: I recommend Vim or VS Code
 * Internet connection
 
### What makes a good API?
* Predictable and consistent
* Static
* Simple and clear
* Flexible: easy to scale and maintain

#### API versioning is mandatory only if:
* You are introducing breaking changes features
* Your clients apps are installable eg on *mobile* or *desktop*
* If other developer will use your api
* If you plan to scale you application
* If you are shipping features gradually

### Things to consider
##### Step 1: Open your terminal and type:

      rails new [your api name] - api

here make sure to leave space between the dash(-) and the api keyword
In this exmple is used sqlite if you don't want to use sqlite a -d to spacify your database

##### Step 2: Versioning your api
We wanna access our api via /api/v1 url. First let's create the missing folders
        
        mkdir app/controllers/api
        mkdir app/controllers/api/v1

##### Step 3: Scaffold a resource
Open terminal and excute Rails commands
  
        rails g scaffold Articles title:string content:text slug:string
  
        rails db:migrate
  
##### Step 4: Move the resource controller to its respective folder 
       
       mv app/controllers/articles_controller.rb app/controllers/api/v1
  
##### Step 5: Update the controller
We will add the namespaces prior (before) the class name (line 1) and update the url helper in the create method
The final result should be something like this

    class Api::V1::ArticlesController < ApplicationController
   
          before_action :set_article, only: [:show, :update, :destroy]
        
           # GET /articles
           def index
              @articles = Article.all
              render json: @articles
           end

           # GET /articles/1
           def show
               render json: @article
           end
         
           # POST /articles
           def create
               @article = Article.new(article_params)
               if @article.save
                  render json: @article, status: :created, location:
                  api_v1_article_url(@article)
               else
                  render json: @article.errors, status: :unprocessable_entity
               end
           end
         
          # PATCH/PUT /articles/1
          def update
              if @article.update(article_params)
                  render json: @article
              else
                  render json: @article.errors, status: :unprocessable_entity
              end
          end
          
          # DELETE /articles/1
          def destroy
              @article.destroy
          end
         
         private
         # Use callbacks to share common setup or constraints
         between actions.
         def set_article
             @article = Article.find(params[:id])
         end
         
         # Only allow a trusted parameter “white list” through.
         def article_params
             params.require(:article).permit(:title, :content, :slug)
         end
       end
       
 #### Step 6: Update the route file
 Open config/routes.rb with your text editor. Delete the resource line 
 
         resource :articles
      
 Then add the namespaces. The final route file should look like the one below
 
          Rails.application.routes.draw do
               namespace :api do
                   namespace :v1 do 
                      resources :articles
                    end
                end
                # For details on the DSL available within this file, see
                http://guides.rubyonrails.org/routing.html
          end
          
 #### Step 7: More resources
 You can repeat the steps (3 - 6) for as many resources as you want
 
 #### Step 8: Try it!
 We will start by seeding the database
 Open db/seeds.rb and create a record
 
       Article.create(title: "Hello World", content: "This is my first api application in rails", slug: "hello-world")
     
 Then run
      
      rails db:seed
      
 Start the server
      
      rails s
      
 Open the browser and go to 
 
      http://localhost:3000/api/v1/articles
      
   or you can use postman if you have installed it
   
   
CONGRATULATIONS!! YOU HAVE MADE IT TO THE END!!!













