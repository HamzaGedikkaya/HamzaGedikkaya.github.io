---
layout: post
title: Gem Devise
subtitle: Devise Nedir? Nasıl Kullanılır?
cover-img: /assets/img/devise.jpeg
thumbnail-img: /assets/img/gem.jpeg
# share-img: /assets/img/devise.jpeg
tags: [Ruby, Rails, GEM, Devise]
---

Kimlik doğrulama ve kullanıcı yönetim işlemlerini kolaylaştıran bir gem'dir. Kayıt olurken alınacak farklı verileri belirleme gibi özellikleri sağlar. Başlıca kullanılan özellikleri şunlardır:
- Kayıt ol
- Oturum aç
- Oturumu kapat
- Şifre değiştir 

### Devise Nasıl Kurulur?

1. Devise gem'ini terminal aracılığı ile hızlı bir şekilde kurmak için şu iki kod yazılır;

    ```
    bundle add devise
    rails generate devise:install
    ```

2. Ardından terminalde de yapılan yönlendirmeler doğrultusunda ***config/environments/development.rb*** içerisine `config.action_mailer.default_url_options = { host ‘localhost’, port: 3000}` eklenir.

3. Kullanıcı modeli oluşturmak ve devise gem'inin varsayılan görünümlerini(views) eklemek için aşağıdaki komutlar çalıştırılır:

    ```
    rails generate devise user
    rails generate devise:views
    ```

4. Son olarak veritabanındaki değişiklikleri tanımlayan Ruby dosyalarını çalıştırmak için ***`rails db:migrate`*** işlemi yapılır.

5. Kayıt olmak için veya oturum açmak için *localhost'da* çalıştırdığımız projemizi görüntülemek için *(http://127.0.0.1:3000/users/sign_up)*, *(http://127.0.0.1:3000/users/sign_in)* sayfalarından birine gidebilir veya aşağıdaki kodlarla direkt olarak bir sayfada buton ve linkler aracılığı ile bu sayfalara erişebilirsiniz.
    ```ruby
    <%= link_to "Sign Up", new_user_registration_path %>
    <%= link_to "Sign In", new_user_session_path %>
    <%= button_to "Sign Out", destroy_user_session_path, method: :delete%>
    <%= link_to "Edit Profile", edit_user_registration_path %>
    ```