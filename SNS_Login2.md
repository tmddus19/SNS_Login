- models/user.rb 파일에

  ![image](https://user-images.githubusercontent.com/40056757/42918516-18c5921c-8b49-11e8-86cf-7de20337b46c.png)

  코드를 넣어준다.

  

- developers.facebook.com에서 페이스북으로 로그인을 한다.

- 앱 만들기를 한후 facebook로그인하기를 누른다.

- 빠른 시작을 하지않고 왼쪽의 설정을 누른다.

  ![image](https://user-images.githubusercontent.com/40056757/42917999-d974990c-8b46-11e8-86d7-05da0f47e795.png)

  

- 유효한 OAuth 리디렉션 URI에 

  ![image](https://user-images.githubusercontent.com/40056757/42918072-27cae30e-8b47-11e8-9f4e-0edd55afbe44.png)

  workspace주소와 뒷부분을 넣어주고 변경내용저장을 누른다.

- 왼쪽의 설정에 기본설정 앱도메인에 workspace의 주소를 넣어준다.

- 기본설정에 있는 앱ID와 앱 시크릿코드를 config/initializers/devise.rb 파일에 넣어준다.

  (앱ID와 앱 시크릿코드는 유출되지 않게 조심한다.)

  페이스북아이디가 전화번호로 되있는경우 로그인을 할 수 없다.

- 앱ID와 앱 시크릿코드를 유출되지 않게 하는법

  ![image](https://user-images.githubusercontent.com/40056757/42921329-8717cb38-8b56-11e8-8ff7-912ef8358890.png)

​      콘솔창 부분에 오른쪽 끝에 있는 ENV를 누른후 Name부분에 `FACEBOOK_APP_ID를 `치고 엔터를 한 후 앱ID를 넣

​      어주고 , Value부분에 `FACEBOOK_APP_SECRET`를 치고 엔터를 한후 앱 시크릿코드를 넣어준다.           

- user.rb omniauth 

- (https://developers.naver.com/main/)에 들어가면 네이버 아이디로 로그인을 만들 수 있다.

- 네이버 아이디로 로그인

- 오픈 api 이용 신청을 누른후 계정동의를 하고나면

  ![image](https://user-images.githubusercontent.com/40056757/42921881-3f73fa56-8b59-11e8-909e-d721f5c67bb7.png)

  이 페이지가 뜬다.

  

- 사용 API를 네이버 아이디로 로그인을 하면 권한 설정을 할 수 있다.

  ![image](https://user-images.githubusercontent.com/40056757/42922030-d8ecd2ca-8b59-11e8-9338-a33079888f96.png)

  

- config/initializers/devise.rb파일에 페이스북 코드 밑에 넣어준다.

  ```ruby
  config.omniauth :naver, ENV["NAVER_APP_ID"], ENV["NAVER_APP_SECRET"]
  ```

  

- models/user.rb파일에 추가해준다.

  ```ruby
  :omniauthable, omniauth_providers: %i[facebook] 
  -> :omniauthable, omniauth_providers: %i[facebook naver] 
  ```

  

- controllers/users/omniauth_callbacks_controller.rb 파일에 페이스북을 했던 코드를 복사해서 붙여넣어준뒤 facebook부분을 naver로 바꿔준다.

  ```ruby
    def naver
      # You need to implement the method below in your model (e.g. app/models/user.rb)
      @user = User.from_omniauth(request.env["omniauth.auth"])
  
      if @user.persisted?
        sign_in_and_redirect @user, event: :authentication #this will throw if @user is not       activated
        set_flash_message(:notice, :success, kind: "naver") if is_navigational_format?
      else
        session["devise.naver_data"] = request.env["omniauth.auth"]
        redirect_to new_user_registration_url
      end
    end
  ```

  

