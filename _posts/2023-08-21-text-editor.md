---
layout: post
title: Text Editor
subtitle: Nedir ve Nasıl Kullanılır?
cover-img: /assets/img/text.png
thumbnail-img: /assets/img/rails.png
# share-img: /assets/img/text.png
tags: [Ruby, Rails, Text Editor]
---

Text Editor yani ***metin düzenleyici***, metin belgelerini düzenlemek ve farklı stiller kazandırarak yazmak için kullanılan bir yazılımdır. MS Word gibi uygulamalardaki üst taraftaki düzenleme çubuğu buna örnek olarak gösterilebilir. Bazı özellikleri aşağıda sıralanmıştır:

- Temel Metin Düzenleme
- Yazı Tipi Değiştirme
- Yazı Boyutu Değiştirme
- Dil Vurgusu
- Satır Numaraları..

### Nasıl Kullanılır

En basit şekilde projemize metin düzenleyiciyi nasıl ekleyeceğimizi göstereceğim. Detaylı bir şekilde görüntülemek ve incelemek için ***[tıklayınız](https://edgeguides.rubyonrails.org/action_text_overview.html)***.

1. Terminalde ***`rails action_text:install`*** ve ***`rails db:migrate`*** komutlarını çalıştırın.

2. ***app/models/message.rb*** (bu dosyayı oluşturun) konumundaki içeriği şu şekilde değiştirin:

    ```ruby
    class Message < ApplicationRecord
        has_rich_text :content
    end
    ```

3. Ardından text editor'u sayfamızda *(new.html.erb veya edit.html.erb)* görüntülemek için, içerik girmekte kullandığımız ***text_area*** kısmını ***rich_text_area*** olarak değiştirelim. Örnek formu aşağıda görebilirsiniz:

    ```ruby
        <%= form_with model: article do |form| %>
    <div class="field">
        <%= form.label :content %>
        <%= form.rich_text_area :content %>
    </div>
    <% end %>
    ```

4. Metin düzenleyicide yaptığımız değişikliklerin uygulanıp görüntülenebilmesi için, makale içeriğinin bulunduğu dosyada *(show.html.erb)*, ***`<%= article.content %>`*** kısmını ***`<%= article.content.html_safe %>`*** olacak şekilde güncellemeliyiz.