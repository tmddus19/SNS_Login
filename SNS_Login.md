# SNS_Login

- Gemfile에 

  ```ruby
  gem 'devise'
  gem 'omniauth-facebook' #facebook으로 로그인하기
  ```

  를 추가한 후 콘솔창에 `bundle`을 해준다.

- 콘솔 창에 `rails g devise:install`

- `rails g devise user`

- `rails g devise:controllers users`

- devise를 수정할 때는 콘솔창에 `rails g devise:views`를 해준다.

- 추가칼럼 db/migrate/20180719012453_devise_create_users 파일에

  ```ruby
  class DeviseCreateUsers < ActiveRecord::Migration
    def change
      create_table :users do |t|
        t.string :uid                              <-추가
        t.string :provider                         <-추가
        t.string :name                             <-추가
        t.string :image                            <-추가
        ## Database authenticatable
        t.string :email,              null: false, default: ""
        t.string :encrypted_password, null: false, default: ""
  
        ## Recoverable
        t.string   :reset_password_token
        t.datetime :reset_password_sent_at
  
        ## Rememberable
        t.datetime :remember_created_at
  
        ## Trackable
        t.integer  :sign_in_count, default: 0, null: false
        t.datetime :current_sign_in_at
        t.datetime :last_sign_in_at
        t.string   :current_sign_in_ip
        t.string   :last_sign_in_ip
  
        ## Confirmable
        # t.string   :confirmation_token
        # t.datetime :confirmed_at
        # t.datetime :confirmation_sent_at
        # t.string   :unconfirmed_email # Only if using reconfirmable
  
        ## Lockable
        # t.integer  :failed_attempts, default: 0, null: false # Only if lock strategy is :failed_attempts
        # t.string   :unlock_token # Only if unlock strategy is :email or :both
        # t.datetime :locked_at
  
  
        t.timestamps null: false
      end
  
      add_index :users, :email,                unique: true
      add_index :users, :reset_password_token, unique: true
      # add_index :users, :confirmation_token,   unique: true
      # add_index :users, :unlock_token,         unique: true
    end
  end
  
  ```

  

- 콘솔창에 `rails g scaffold post user_id:integer title content image`를 해준다.

- 콘솔창에 `rake db:migrate`를 해준다.

- 프로젝트를 켜본다.

- config/routes.rb파일에서 ``root 'posts#index'`를 추가해준다.

- views/ layouts/ application.html.erb파일에

  ```ruby
  <!DOCTYPE html>
  <html>
  <head>
    <title>Workspace</title>
    <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track' => true %>
    <%= javascript_include_tag 'application', 'data-turbolinks-track' => true %>
    <%= csrf_meta_tags %>
  </head>
  <body>
  
    <% if user_signed_in? %>                                               <-추가
      <span><%= current_user.email %></span>                                <-추가
      <%= link_to 'logout', destroy_user_session_path, method: :delete %>  <-추가
    <% else %>
      <%= link_to 'login', new_user_session_path %>
    <% end %>
      
    <% end %>
  
  <%= yield %>
  
  </body>
  </html>
  ```

  

- 구글에 rails devise facebook login을 검색해서 찾아본다.

  ![image](https://user-images.githubusercontent.com/40056757/42916499-7839ece2-8b40-11e8-9fbd-6d6cf47ef26d.png)

  

- 밑출진 코드를

  ![image](https://user-images.githubusercontent.com/40056757/42916573-f014c9bc-8b40-11e8-9e9d-4d4c850ffd77.png)

  

- config/initializers/devise.rb파일에  코드를 붙여준다.

  ![image](https://user-images.githubusercontent.com/40056757/42916596-09c65c36-8b41-11e8-8c83-8ac64e9af633.png)

  

- `%i`루비에서 제공하는 배열을 만드는 방법중 하나 ex)  `%i[facebook naver] #=> [:facebook, :naver]`

  `%w`  ex) `%w[facebook naver] #=> ['facebook','naver']`

-  밑의코드를 

  ![image](https://user-images.githubusercontent.com/40056757/42916804-e8c0924e-8b41-11e8-992d-c3fc2f08987a.png)

- app/models/user.rb에 추가해준다.

  ![image](https://user-images.githubusercontent.com/40056757/42916795-dbe0f596-8b41-11e8-9af6-1aacfb8a4ddf.png)

  

- 다시 프로젝트를 켜보면

  ![image](https://user-images.githubusercontent.com/40056757/42917314-fd1f4b52-8b43-11e8-861c-c98dc9542aea.png)

  이렇게 나온다.

- config/routes.rb 파일에 코드를 추가해준다.

  ![image](https://user-images.githubusercontent.com/40056757/42917373-38a734dc-8b44-11e8-89a4-4e8c8cc35efe.png)

  

- controllers/users/omniauth_callbacks_controller 에

  ![image](https://user-images.githubusercontent.com/40056757/42917677-8a836ab8-8b45-11e8-8f4a-9a58885452ce.png)

  코드를 넣어준다.

- 

- 

- 