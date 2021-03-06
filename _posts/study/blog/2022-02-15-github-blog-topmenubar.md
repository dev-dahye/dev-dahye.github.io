---
layout: post
title: "탑메뉴바와 검색기능 추가"
tags: gitblog top-menu-bar search
image: 
    path: /assets/img/blog/gitBlog.png
accent_color: '#01ADB5'
description: >
    깃허브 블로그 만들기: 탑메뉴바와 검색기능 추가
categories:
    - study
    - blog
related_posts:    
    -    
---
# [Blog] 탑메뉴바와 검색기능 추가
* 
{:toc}

## 탑메뉴바 추가
#### 1. html 추가
**_includes > body** 에 menu.html 추가

```html
<!--상단메뉴바-->
<div id="_navbar" class="navbar fixed-top">
    <div class="content">
        <span class="sr-only">{{ site.data.strings.jump_to | default:"Jump to" }}{{ site.data.strings.colon | default:":" }}</span>
        <div class="nav-btn-bar">
            <a id="_menu" class="nav-btn no-hover" href="#_drawer--opened">
                <span class="sr-only">{{ site.data.strings.navigation | default:"Navigation" }}</span>
                <span class="icon-menu"></span>
            </a>
            <!--상단메뉴 카테고리들 -->
            <div class="top-menu">
                {% if site.menu %}
                {% for node in site.menu %}
                {% assign url = node.url | default: node.href %}
                <a {% if forloop.first %}id="_drawer--opened" {% endif %} href="{% include_cached smart-url url=url %}" class="nav-btn top-menu {% if node.external %}external{% endif %}" {% if node.rel %}rel="{{ node.rel }}" {% endif %}>
                    {{ node.name | default:node.title }}
                </a>
                {% endfor %}
                {% endif %}
            </div>

            <!--검색 tipuesearch -->

        </div>
    </div>
</div>
<hr class="sr-only" hidden />
```

#### 2. css 추가
**_sass > my-style.scss** 아래에 다음코드를 추가

```scss
// for top-menu
.top-menu-wrapper{
  display: inline-block;
}
.top-menu{
  overflow-x: auto; // overflowx 속성은 x 축, 즉 왼쪽과 오른쪽의 내용이 더 길 때(가로),
  overflow-y: hidden; // overflowy 속성은 y 축, 즉 위와 아래의 내용이 더 길때 (세로) 어떻게 보일지 선택하는 속성
  white-space: nowrap; // whitespace 는 스페이스와 탭, 줄바꿈, 자동줄바꿈을 어떻게 처리할지 정하는 속성, nowrap :병합
}
// for top-menu
.top-menu.nav-btn{
  border: none;
  display: inline-flex; // Flex 는 요소의 크기가 불분명하거나 동적인 경우에도, 각 요소를 정렬할 수 있는 효율적인 방법을 제공, Inline 특성의 Flex Container 를 정의
  width: 6rem; // inlineflex : Inline(Inline Block) 요소와 같은 성향(수평 쌓임)
  margin-left: -1px;
}
```

#### 3. 탑메뉴바 완성
![탑메뉴바](/assets/img/blog/topmenubar1.png){:width="70%" height="70%"}

---

## 검색기능 추가
블로그내에 검색기능을 추가하여 원하는 게시물을 빠르게 찾아보자.
### 1. 다운로드
다음 [Github 레파지토리](https://github.com/jekylltools/jekyll-tipue-search)에서 zip 파일을 다운로드 받아 압축을 푼다.   

### 2. 검색페이지
**search.html**을 gitBlog폴더 root directory에 붙여넣는다.    
![download/search.html](/assets/img/blog/search1.png){:width="35%" height="35%"}➡️
![gitBlog/search.html](/assets/img/blog/search2.png){:width="40%" height="40%"}
 
### 3. css 추가
다운로드 폴더의 /assets/tipuesearch를 gitBlog/assets/에 붙여넣는다.
![download/assets/tipuesearch](/assets/img/blog/search3.png){:width="40%" height="40%"}➡️
![gitBlog/assets/tipuesearch](/assets/img/blog/search4.png){:width="40%" height="40%"}

### 4. 검색 설정
**_config.yml**에 가장 밑에 추가   
```yaml
tipue_search:
  include:
    pages: false
    collections: []
  exclude:
    files: []
    categories: []
    tags: []
```
include 부분의 pages: false의 설정은 pages 레이아웃에 해당하는 일반 html페이지는 검색하지 않겠다는 것을 의미한다.(포스트 내용 검색에 집중하기 위함)
exclude 부분의 search.html, index.html, tags.html 페이지는 검색에서 제외하겠다는 것을 의미한다.   

### 5. html 추가
**_includes > body > menu.html** `<!--상단메뉴 카테고리들-->` 밑에 추가   

```html
<!--검색 tipuesearch -->
<div class="nav-span">
    <form action="/search">
        <div class="tipue_search_left">
            <img src="/assets/tipuesearch/search.png" class="tipue_search_icon">
        </div>
        <div class="tipue_search_right">
            <input type="text" name="q" id="tipue_search_input" pattern=".{1,}" title="At least 1 characters" required>
        </div>
        <div style="clear: both;"></div>
    </form>
</div>
```

### 6. css 적용
**include > my-head.html** 가장 밑에 추가     

```html
<!-- tipuesearch -->
<link rel="stylesheet" href="/assets/tipuesearch/css/tipuesearch.css">
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
<script src="/assets/tipuesearch/tipuesearch_content.js"></script>
<script src="/assets/tipuesearch/tipuesearch_set.js"></script>
<script src="/assets/tipuesearch/tipuesearch.min.js"></script>
```

### 7. 검색창 생성   
![검색창](/assets/img/blog/search5.png){:width="90%" height="90%"}

### 8. 검색창 꾸미기
- **_sass>my-style.scss** 에 다음 코드 추가, 탑메뉴바가 너무 넓어서 높이를 줄이자.

```css
.nav-btn-bar{
  height: 3rem;
}
```

- **assets>tipuesearch > css > tipuesearch.css**의 #tipue_search_input에서 다음과 같이 수정

```css
#tipue_search_input
{
     color: #333;
     max-width: 150px;		    /*최대 넓이 줄이고*/
     max-height: 20px;		    /* 높이 지정*/
     padding: 15px;			    /* 패딩 줄이기*/
     border: 2px solid #626591;	/* 굵게, 테두리는 색 바꾸기*/
     border-radius: 0;
     -moz-appearance: none;
     -webkit-appearance: none;
     box-shadow: none;
     outline: 0;
     margin: 0;
     text-align: center;		/*텍스트 중간에 오게하기*/
}

/* 돋보기 아이콘 위치 수정*/
.tipue_search_icon
{
     width: 19px;
     height: 19px;
     margin-bottom: 1rem;
}

/* 검색창 위치 조정*/
.tipue_search_left
{
     float: left;
     padding: 10px 5px 0 0;
     color: #170247;
     max-height: 20px;
}
```

<br>
<br>

- - -

## Reference 
- [면접관이 좋아하는 Git Portfolio 만들기 (with_GitBlog)](https://projectlion.io/courses/technology/gitblog)