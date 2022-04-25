## 기술 블로그 제작기

- Jekyll[(공식사이트)](https://jekyllrb-ko.github.io/)를 이용해서 Blog 제작

- 배포 : 🚀 github-pages

- 블로그 주소 : https://lhk3337.github.io/

- 블로그는 [식빵맘](https://ansohxxn.github.io/blog/i-made-my-blog/)님 블로그를 참조해서 완성했습니다.

- 카테고리 및 post 추가 하는 법

  - \_pages - categories에 category-nextjs.md 생성

    ```
        ---
        title: "nextJS"
        layout: archive
        permalink: categories/nextjs
        author_profile: true
        sidebar_main: true
        ---
        ---
        {% assign posts = site.categories.nextjs %}
        {% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}
    ```

  - \_include - nav_list_main에서

    ```html
    <ul>
      {% for category in site.categories %} {% if category[0] == "nextjs"%}
      <li>
        <a href="/categories/nextjs" class="">NextJS ({{category[1].size}})</a>
      </li>
      {% endif %} {% endfor %}
    </ul>
    ```
