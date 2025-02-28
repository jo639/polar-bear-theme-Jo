# Master 브랜치가 아닌 Main 브랜치로 확인, 평가해주세요!


## 블로그 시작 환경 만들기

#### ruby 및 jekyll 설치
Github Blog 를 만들고 테마를 적용하여 나만의 블로그를 만들어야 했다.  학교에서 지급된 우분투 환경에는 기본적으로 ruby가 설치되어 있었고, 지킬 홈페이지의 가이드에 따라 jekyll 설치를 진행했다. 나는 별다른 error 없이 순탄하게 설치가 완료되었고, 곧바로 실습에 들어갔다.
{% highlight ruby %}
ruby -v
gem -v
jekyll -v
{% endhighlight %}
위 명령을 통해 제대로 설치되어있는지 확인하였다.

## 블로그 페이지 구현하기

#### Github와 연동하기
{% highlight ruby %}
git clone 내 주소 GitBlog
{% endhighlight %}
를 통해서 내 로컬 저장소에 디렉토리를 만들고
{% highlight ruby %}
jekyll new . --force
{% endhighlight %}
로 디렉토리에 jekyll을 설치했다.
{% highlight ruby %}
jekyll serve
{% endhighlight %}
를 실행하면 출력되는 주소 **localhost:4000**에 접속하여 첫 블로그 페이지 구현을 완료했다!

## 첫 포스팅 업로드하기

#### Post Upload 방식 알아보기
포스트 업로드는 _posts 폴더에서 할 수 있다.
파일 이름은 날짜-제목 형태로
{% highlight ruby %}
YYYY-MM-DD-Title.markdown
{% endhighlight %}
다음과 같이 이름을 지정한다.

#### Markdown 형식 알아보기
문서의 시작 부분에서 다음과 같은 형식으로 속성을 지정한다.
{% highlight ruby %}
layout: post
title: "title"
date: YYYY-MM-DD
publised: true
categories: 
{% endhighlight %}
그 아래로는 마크다운의 문법에 맞추에 글씨의 크기, 기울기, 강조 등을 변경하여 글을 작성할 수 있다.
또한 링크나 이미지, 표, 코드 블록, 각주 등도 사용 가능하다.
[마크다운 문법 참고 링크](https://velog.io/@bluewind8791/Markdown-Kramdown)

#### Local에서 Remote로 반영하기
현재는 로컬 저장소에만 저장되었는 변경 정보를 원격저장소로 전송하는 과정이다.
파일을 수정했다면 jekyll serve를 통해 확인해보자. 올바르게 변경된 것을 알 수 있다.
그렇지만 본인의 Github Page (*username*.github.io)에 접속하였을 때, 올바르게 반영되지 않는다.
아직 원격저장소에 반영하지 않았기 때문!
{% highlight ruby %}
git add 수정한 파일
git commit -m "커밋내용"
git push origin main
{% endhighlight %}
을 통해 원격저장소에 push까지 완료해준다.
이후 *username*.github.io 접속하면!
반영되어야 하지만...

그런데 새로운 포스트가 업로드되지 않았다?

#### 포스팅이 반영되지 않는 에러 해결하기
##### 날짜 확인하기
원인을 탐색한 결과 _posts에서 파일을 생성할 때 지정한 날짜가 당시 날짜보다 나중이라는 것.
즉, 미래를 날짜로 지정한 경우 포스트가 정상적으로 업로드되지 않을 수 있음을 발견하였다.
(지정한 날짜와 업로드한 날짜가 같을지라도 pc의 시간이 다른 나라로 설정되어 있는 경우, 시차에 따라 같은 에러가 발생할 수 있다고 한다.)
이를 해결하기 위해, _config.yml 에서
{% highlight ruby %}
future: true
{% endhighlight %}
라는 코드를 추가하였다.

###### 그런데도 해결되지 않는다면..?
내 경우에는 위 코드 한줄을 추가했을 때에도 포스트가 추가되지 않았다.
나는 포스트 문서의 시작부분 형식을 지정하는 곳에
{% highlight ruby %}
published: true
{% endhighlight %}
를 추가함으로써 해결하였다.
원래 기본적으로 published는 true로 되어있다고 하는데 어떠한 이유에서인지 올바르게 작동하지 않은 것으로 예상된다.

# 블로그 테마 적용하기

#### 테마 적용 방법
다양한 무료 테마 사이트가 존재하고, 자신의 블로그의 목적에 맞는 테마를 선택한다.  
테마를 적용하는 방식은 다양한데,
1. 기존 블로그에 테마 적용하기: ```git clone``` 으로 로컬에 받아온 후, _posts를 제외한 파일에 테마를 덮어쓰고 push 하기
2. 처음부터 테마를 적용하여 만들기: 원하는 테마의 원격 저장소에서 fork 해온 후, ```git clone```으로 받아오기  

두 가지 방법 중 나는 2번째 방법을 사용했다.

#### 테마 적용 실습!
나는 simple하면서 색감이 마음에 들어 polar-bear 테마를 선택했다.  
내 저장소로 fork 해온 후,
```git clone 내 주소 GitBlog```하여 로컬 저장소로 가져왔다.
```jekyll serve```를 진행한 결과, 올바르게 페이지가 출력되었다.
그러나 원격저장소에 제대로 push 했음에도 *username*.github.io에 접속하였을 때,  
페이지가 깨지는 현상을 발견했다.

#### 테마 원격저장소에 반영하기
원격 저장소에 반영되지 않는 원인은 _config.yml에서 찾을 수 있었다.
baseurl과 url을 올바르게 설정하지 않아 페이지가 제대로 연결되지 않은 것 같다.

![baseurl](https://kairos03.github.io/assets/img/posts/jekyll/2017-09-11-learing-Up-Confusion-Around-baseurl/1.png)  

내 주소에서 baseurl 부분과 url 부분을 찾아 _config.yml에 다음과 같이 입력해주었다.
{% highlight ruby %}
baseurl: "/polar-bear-theme/" # the subpath of your site, e.g. /blog/
url: "https://jo639.github.io" # the base hostname & protocol for your site
{% endhighlight %}

> 그런데 자꾸 테마의 데모페이지가 반영되는 문제를 발견했다.
내가 처음 fork 해올 때 저장소의 이름을 바꾸지 않고 가져와서 baseurl이 같은 탓에 연결이 꼬인 것으로 보였다.
그래서 repository 이름을 변경해주고 변경한 주소에 맞게 baseurl을 다시 입력했더니 완전히 해결되었다.  

### 이로써 테마 적용까지 성공하였다!

## 댓글 기능 구현하기

우선 [Disqus 홈페이지](https://disqus.com/)에 접속한다.   

[다음 절차](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/c892cead-42b2-4e27-bf24-f84a0d5630f6/blog_four.pdf?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221128T075035Z&X-Amz-Expires=86400&X-Amz-Signature=d244db4041bca277c2f3c0abfcafb6f31824ca74e372df2a27e71d51998f2dd3&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22blog_four.pdf%22&x-id=GetObject)에 따라 가입과 세팅을 진행한다.  

주의할 점은 _config.yml에
```
comment:
    provider: "disqus"
    disqus:
        shortname: "jo639"
```
와 같이 key-value를 추가해야 한다는 것이다.  
또한 _layouts/post.html 파일에서 다음과 같은 코드를 추가하여 push 해준다.
```
{% if page.comments %}
</div>
<div id="disqus_thread"></div>
<script>
    /**
    *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
    *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables    */
    
    let PAGE_URL = "{{site.url}}{{page.url}}"
    let PAGE_IDENTIFIER = "{{page.url}}"
    var disqus_config = function () {
    this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
    this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
    };
    (function() { // DON'T EDIT BELOW THIS LINE
    var d = document, s = d.createElement('script');
    s.src = 'https://jo639.disqus.com/embed.js';
    s.setAttribute('data-timestamp', +new Date());
    (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
{% endif %}
```
  
이후 댓글 기능을 사용하고 싶은 포스트에만 ```comments: true```를 추가해준다.