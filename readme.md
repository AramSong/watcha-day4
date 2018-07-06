# Watcha - 4일차



### Pagination - kaminari

```ruby
gem 'kaminari'
gem 'bootstrap4-kaminari-views'
```

`movies_controller.rb`

```ruby
  def index
    @movies = Movie.page(params[:page])	#1이면 1페이지,2면 2페이지.
      								  #한페이지당 25개씩보여줌.
  end

```

`app/models/movie/rb`

```ruby
class Movie < ApplicationRecord
    mount_uploader :image_path,ImageUploader
    has_many   :likes
    has_many   :users,through: :likes
    has_many   :comments
    
    paginates_per 8
end
```

```ruby
 Movie Load (0.3ms)  SELECT  "movies".* FROM "movies" LIMIT ? OFFSET ?  [["LIMIT", 8], ["OFFSET", 56]]
#LIMIT은 한번에 몇개인지, OFFSET은 총 몇개인지
```

```ruby
@users = User.order(:name).page params[:page] #order(:name) name컬럼 기준으로 정렬
```

`index.html.erb`

```erb
<%=paginate @movies,theme: 'twitter-bootstrap-4'%>
```

#### carousel/#slides-only : 슬라이드형식의 카드. bootstrap

### 검색창 만들기

* input창에 글자를 한글자 입력할 때마다
* server로 해당 글자를 검색하는 요청을 보내고
* 응답으로 날아온 영화제목 리스트를 화면에 보여준다.

```ruby
2.4.0 :001 > "".strip.empty?
 => true 
2.4.0 :002 > "".empty?
 => true 
2.4.0 :003 > " ".empty?
 => false 
2.4.0 :004 > " ".strip.empty?
 => true 
```

`respond_to do |format|`: format에 따라 다른 형식의 파일으로? 분기가 달라서 서로 다른 자바스크립트 파일을 보내줘야 할 경우, to do |foramt|에 가변수 format을 통해서 `format.js {render 'no_content'}`자바스크립트 파일 'no_content ' 를 찾아서 보내줘라.

ex) 일반 문서는 html로 json은 js문서로 보내라.

```ruby
        format.html { redirect_to @movie } 
        format.json { render :show, status: :ok, location: @movie }
```



없을 경우 액션명이 일치하는 파일을 자동으로 찾아서 보내줬다.  액션명과 일치하는 자바스크립트 파일을 만들어줌.