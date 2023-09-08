---
layout: post
title: Gem Rolify
subtitle: Rolify Nedir? Nasıl Kullanılır?
cover-img: /assets/img/rolify.jpeg
thumbnail-img: /assets/img/gem.jpeg
# share-img: /assets/img/rolify.jpeg
tags: [Ruby, Rails, GEM, Rolify]
---

*Rolify* bir Ruby on Rails uygulamasında rol (yetki) tabanlı yetkilendirme sistemi oluşturmanıza yardımcı olan bir Ruby gem'idir. Bu gem, kullanıcıların ve kaynakların (örneğin, bir makale veya bir fotoğraf albümü gibi) belirli rollerle ilişkilendirilmesini yönetmeyi kolaylaştırır. Temel işlevleri şu şekilde sıralanabilir:

- Rol Tanımlama
- Kullanıcılarla Rollerin İlişkilendirilmesi
- Kaynaklarla Rollerin İlişkilendirilmesi 
- Yetkilendirme Kontrolleri

### Kurulum

1. ***`gem "rolify"`*** gemfile dosyasının içine eklenir ve terminalde ***`bundle install`*** çalıştırılır.

2. Terminale ***`rails g rolify Role User`*** yazılıp model oluşturulur ve `rails db:migrate` işlemi yapılarak database'e kaydedilir.

3. Bundan önce ***Devise Gem*** kullanarak oluşturduğumuz kullanıcılardan bir tanesine bir rol verebiliriz; Rails console'a yazılan:
    - `User.all` tüm kullanıcıları görüntülememizi sağlar.
    - `User.first.add_role :admin` listelenen ilk kullanıcıya admin rolü tanımlar.
    - `User.first.has_role? :admin` ilk kullanıcı admin mi sorusunu sorar.

      ![background](/assets/img/30px.png)


4. index sayfasında mevcut giriş yapmış olan kullanıcının rolünü göstermek için şu if bloğunu kullanabiliriz;
    ```ruby 
      <% if user_signed_in? %>
        <%= current_user.roles.pluck(:name) %>
      <% end %>
    ```
    ![background](/assets/img/30px.png)

5. Yeni kayıt olan kullanıcının rolünü ayarlamak için ise ***User.rb*** içerisine *default_role* tanımlayabiliriz

    ```ruby 
      after_create :assign_default_role

      def assign_default_role
        self.add_role(:newuser) if self.roles.blank?
      end
    ```

![background](/assets/img/30px.png)

### Kullanıcıları Görüntüleme ve Rol Ekleme

1. ***users_controller.rb***
  ```ruby
    class UsersController < ApplicationController
      def index
        @users = User.order(created_at: :desc)
      end
    end
  ```

2. ***user.rb***
  ```ruby
    class User < ApplicationRecord
      rolify
      devise :database_authenticatable, :registerable,
            :recoverable, :rememberable, :validatable
      after_create :assign_default_role
      validate :must_have_a_role, on: :update

      private

      def must_have_a_role
        unless roles.any?
          errors.add(:roles, "muts have at least 1 role")
        end
      end

      def assign_default_role
        self.add_role(:newuser) if self.roles.blank?
      end
    end
  ```

3. ***users/index.html.erb***
  ```ruby
    <% @users.each do |user| %>
      <%= user.email %>
      <%= user.roles.pluck(:name) %>
      <br>
    <% end %>
  ```

4. ***users/edit.html.erb***
  ```ruby
      <%= form_with(model: @user) do |form| %>
      <%= form.collection_check_boxes :role_ids, Role.all, :id, :name%>
      <%= @user.errors[:roles]%>
      <%= form.button :submit%>
      <% end %>
  ```
  ![background](/assets/img/30px.png)


### Rolleri Yetkilendirme

- Bir modelin yetkilendirmeyle yönetilmesini sağlamak için ***article.rb*** içerisine ***`resourcify`*** eklenir.

- Bir yetki vermek istediğimizde, örneğin bir makaleyi kaydetme özelliği vermek istersek ***article_controller.rb*** *save* kısmına ***`current_user.add_role :author, @article`*** gibi bir satır ekleyebiliriz.

-***article.rb*** 
  ```ruby
    has_many :users, -> { distinct }, through: :roles, class_name: 'User', source: :users
    has_many :editors, -> { where(:roles => {name: :editor}) }, through: :roles, class_name: 'User', source: :users
  ```
  ![background](/assets/img/30px.png)

- Rolleri ve kullanıcıları görüntülediğimiz ve rol atayabileceğimiz, değiştirme yapabileceğimiz sayfayı sadece adminin erişimine açmak için ***users_controller.rb*** içerisine;
  ```ruby
    before_action :require_admin, only: [:index, :edit, :update]

    def require_admin
      unless current_user && current_user.has_role?(:admin)
        flash[:alert] = "Bu sayfaya erişim izniniz yok."
        redirect_to root_path
      end
    end
  ```

  ve bu konuma gidecek butonun sadece *admin* giriş yaptığında görünebilir olması için:
    ```ruby
      <% if current_user && current_user.has_role?(:admin) %>
        <li class="nav-item">          
          <%= link_to "Admin Paneli", users_path, class: "nav-link" %>
        </li>
      <% end %>
    ```
    ![background](/assets/img/30px.png)

- New, create, edit, update, destroy gibi methodlara sadece yazarın ve adminin erişmesini ve kullanmasını istediğimizde *articles_controller*'a 
  ```ruby
    before_action :require_author_or_admin, only: [:new, :create, :edit, :update, :destroy]

    private

      def require_author_or_admin
        unless current_user.has_role?('author') || current_user.has_role?('admin')
          flash[:alert] = 'Bu işlem için yetkiniz yok.'
          redirect_to root_path
        end
      end
  ```

  ve bu sayfalara yönlendiren butonların da sadece admin ve yazar tarafından görünmesini istediğim için linklere, butonlara şu *if* bloğunun benzeri eklenebilir:
  ```ruby
    <% if current_user&.has_role?(:admin) || current_user&.has_role?(:author) %>
      <%= link_to "+", new_article_path, class: "new" %>
    <% end %>
  ```
  ![background](/assets/img/30px.png)

- Yazar tarafından oluşturulan makalenin başlangıçta görünmemesi, editor tarafından onaylandıktan sonra görünmesini sağlayan bir işlev kazandırmak istediğimizde öncelikle

- Oluşturulan makaleleri *article.rb* içerisinde, ***active*** sınıfını ***false*** olarak tanımlayıp, index'de sadece onaylanmış olanlar yani ***active: true*** olanların görüntülenmesi için controller'ı düzenleyebiliriz:
  ```ruby
    def index
      @articles = Article.where(active: true)
    end
  ```

- Aktif olmayanları görüntüleyebilmemiz için

  - controller içerisine:
    ```ruby
      before_action :inactive, only: [:inactive]

      def inactive
        @articles = Article.where(active: false)
        unless current_user&.has_any_role?(:admin, :editor)
          flash[:alert] = 'Bu işlem için yetkiniz yok.'
          redirect_to root_path
        end
      end
    ```

  - routes.rb içerisine:
    ```ruby
      get :inactive, on: :collection
    ```

  - inactive.html.erb içerisine gidebileceğimiz link:
    ```ruby
      <% if current_user&.has_any_role?(:admin, :editor) %>
        <li class="nav-item">          
          <%= link_to "Editor Panel", inactive_articles_path, class: "nav-link" %>
        </li>
      <% end %>
    ```

- Yayına alma işlemini yapmak, yani aktif değeri false olan makaleyi true değerine döndürmek için:
  - controller'a:
    ```ruby
      def activate
        @article = Article.find(params[:id])
        @article.update(active: true)
        redirect_to inactive_articles_path, notice: 'Makale başarıyla aktifleştirildi.'
      end
    ```

  - routes.rb'ye:
    ```ruby
      get :activate, on: :member
    ```

  - inactive.html.erb'ye:
    ```ruby
      <%= link_to 'Makaleyi Aktifleştir', activate_article_path(article), method: :put, class: 'btn btn-success' %>
    ```

    yapılarak tüm roller tanımlanmış ve işlevlendirilmiş olur.
  