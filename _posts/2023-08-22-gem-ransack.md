---
layout: post
title: Gem Ransack
subtitle: Ransack Nedir? Nasıl Kullanılır?
cover-img: /assets/img/search.jpeg
thumbnail-img: /assets/img/gem.jpeg
# share-img: /assets/img/search.jpeg
tags: [Ruby, Rails, GEM, Ransack]
---

Tasarlanma amacı veritabanı sorgularını oluşturmak ve işlemektir. Web uygulamamız üzerinden filtre ve sıralama işlevlerini bize sunar. Çoğunluklu olarak arama yapmada _**(search)**_ kullanılır. 

### Ransack Nasıl Kurulur?

1. Proje klasörümüz içindeki **_Gemfile_** dosyasına gidip en alt kısma **`gem 'ransack'`** yazılmalı, ardından terminalde **`bundle install`** komutu çalıştırılmalıdır. Bu, gem'i yüklememizi sağlayacaktır.

2. ***models/article.rb*** dosyasının içerisine (farklı bir proje yapıyorsanız, projenizin model dosyası içerisine) 

    ```ruby
    class Article < ApplicationRecord

        def self.ransackable_attributes(auth_object = nil)
            ['title', 'content']
        end

        def self.ransackable_associations(auth_object = nil)
            []
        end
    end
    ```
    satırları eklenmelidir. 

    >İlk tanımladığımız method(def), hangi sütunlarda arama yapılabileceğini belirtir. Tanımladığımız satırda **_title_** ve **_content_** sütunlarında arama yapabileceğimizi belirtiyor. 

    >İkinci satırda ise dize boş bırakıldığı için modeldeki ilişkiler üzerinden Ransack sorgusu yapma engellenmiş olur. Boş bırakılma sebebi güvenlik ve performansa dayanabilir.

3. Bu adımda yapmamız gereken ***app/controllers/articles_controller.rb*** dosyasını güncellemek olacaktır.

    ```ruby
    class ArticlesController < ApplicationController
        def index
            @articles = Article.all
            @q = Article.ransack(params[:q])    
            @articles = @q.result(distinct: true) 
        end
    end
    ```     

    Kullanılan terimleri kısa bir şekilde tanımlamak gerekirse
    >- ***q*** tanımladığımız değişkeni ifade eder ='den sonrasını ***q*** yazarak kullanmamızı sağlar.
    - ***Article*** sorgunun uygulanacağı veritabanı tablosunu temsil eder.
    - ***.ransack*** gem'i kullanarak bir sorgu nesnesi oluşturmada kullanılır.
    - ***params[:q]*** sorgu formuna girilen terimleri içerek parametredir. Ransack'ın işlemesi için kullanılır.
    - İkinci satırdaki ***result*** sonuçları almamızı, ***distinct*** sonuçların benzersiz(tekil) olmasını sağlar.

4. ***app/views/articles/index.html.erb*** dosyamızın içine arama yapabilmek için bir arayüz oluşturalım

    ```bash
    <%= search_form_for @q  do |form| %>
      <%= form.search_field :title_or_content_cont, placeholder: "Arama..." %>
      <%= form.button "Ara", type: 'submit' %>
    <% end %>
    ```
    ![background](/assets/img/30px.png)

5. Sonuçları görüntüleyebilmek için ise ***index*** içinde hali hazırda bulunan şu kod yeterlidir;

    ```ruby
    <% @articles.each do |article| %>
    <%= render article %>
    <p>
      <%= link_to "Show this article", article %>
    </p>
    <% end %>
    ```
    ![background](/assets/img/30px.png)

6. Ayrıca karakter girilmediğinde, beş karakterden az girildiğinde uyarı almak için ***if*** bloğu kullanabiliriz. Düzenlenmiş controller dosyamız şu şekilde olacaktır;

    ```ruby
    def index
        @articles = Article.all
        
        @q = Article.ransack(params[:q])
        
        if
        params[:q].present? && params[:q][:title_or_content_cont].length > 0 && params[:q][:title_or_content_cont].length < 5
        flash.now[:alert] = "5 karakterden az yazamazsın!"
        elsif
        params[:q].present? && params[:q][:title_or_content_cont].blank?
        flash.now[:alert] = "Hiç karakter girmediniz!"
        else
        @articles = @q.result(distinct: true).order(:title).to_a
        end    
    end
    ```