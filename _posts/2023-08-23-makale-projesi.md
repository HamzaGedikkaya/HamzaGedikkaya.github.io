---
layout: post
title: İlk Rails Projesi
subtitle: Makale Projesi - CRUD ve Gem Nedir?
cover-img: /assets/img/article.jpg
thumbnail-img: /assets/img/rails.png
# share-img: /assets/img/article.jpg
tags: [Ruby, Rails, CRUD, GEM, Ransack, İmage Processing]
---

Bu içerikte Rails'i kullanarak ilk projemizi, "Makale Projesini" oluşturacağız. Başlamadan önce şu iki terime bakalım;
- CRUD Nedir?
- Gem Nedir?

![background](/assets/img/30px.png)

### CRUD Nedir?

CRUD **"Create - Read - Update - Delete"** anlamına gelir. Verilerin yönetimi sürecindeki temel dört işlemi; **"Ekleme - Okuma - GÜncelleme - Silme"**, ifade eder.
- Create(Oluşturmak): Veritabanına, uygulamaya veri ekleme anlamına gelir.
- Read(Okumak): Hali hazırda bulunan veriyi sorgulama, görüntüleme işlemidir.
- Update(Güncellemek): Varolan veriyi değiştirme, güncelleme işlemidir.
- Delete(Silme): Varolan veriyi kaldırma işlemidir.

![background](/assets/img/30px.png)

### Gem Nedir?

Kullanıdığımız Ruby dilinde kütüphanelerin, paketlerin dağıtım ve yönetimini sağlayan bir sistemdir. Herhangi bir projeye bir işlevsellik katmak istediğimizde, bunu kısayoldan bize sağlayan, başkaları tarafından geliştirilmiş olan kodlar _**Gem**_'lerdir. Projemizde kullandığımız gem'ler şunlardır;
- Ransack (Arama çubuğu)
- İmage Processing (Fotoğraf Ekleme - Görüntüleme)
- Devise (Kullanıcı İşlemleri)
- Bootstrap (CSS Kütüphanesi)

![background](/assets/img/30px.png)

## Projenin Oluşturulması

1. Rails'de proje oluşturmak için terminale ```rails new```, ardından proje ismi ve kullanacağımız veritabanı girilir;

    >```ruby
    rails new project_name -d postgresql    
    ```                             

    Ardınan veritabanını projeye eklemek için proje konumundayken `rails db:create`, ardından `rails db:migrate` komutları girilir.

    ![background](/assets/img/30px.png)

2. Makale projemize CRUD eklemek için iki yolumuz vardır. Birinci yol hızlı bir şekilde temel CRUD işlemlerini gerçekleştirmemizi sağlayan otomatik kod üretme aracı olan _**scaffold**_'dur. İkinci yol ise her kısmın tek tek oluşturulup düzenlenmesidir. 

    1.YOL >> `rails generate scaffold Article title:string content:text`

    2.YOL >> Öğrenme sürecinde önerilen yoldur. İçeriğin devamında görüntüleyebilirsiniz. ↓↓↓
    
    ![background](/assets/img/30px.png)

## Projeye CRUD Eklenmesi

1. Aşağıdaki kod bloğunda öncelikle **Article** model'i başlık ve içerik olacak şekilde oluşturulur. Sonrasında CRUD işlemlerini yapabileceğimiz metotlar controller'a eklenir.

    >```ruby
    rails generate model Article title:string content:text
    rails generate controller Articles index show update create destroy
    ```

2. `config/routes.rb` konumuna eklenmiş olan get metotları yerine (opsiyonel) `resources :articles` kullanılabilir.

3. CRUD sisteminin çalışması için `app/controllers/articles_controller.rb` dosyasının içeriğinin şu şekilde değiştirilmesi gerekir.

```ruby
class ArticlesController < ApplicationController
  before_action :set_article, only: [:edit, :update, :destroy, :show]
  def index
    @articles = Article.all
  end

  def show
  end

  def new
    @article = Article.new
  end

  def create
    @article = Article.new(article_params)

    if @article.save
      redirect_to @article
    else
      render :new
    end
  end

  def edit
  end

  def update
    @article.update(article_params)

    redirect_to article_path(@article)
  end

  def destroy
    @article.destroy

    redirect_to articles_path
  end

  private

  def article_params
    params.require(:article).permit(:title, :content)
  end

  def set_article
    @article = Article.find(params[:id])
  end
end
```

![background](/assets/img/30px.png)
#### Kısaca anlatmak gerekirse yapılan işlemler:

- `def index` içerisine yazılan satır veritabanındaki tüm makale kayıtlarını çeker ve bu verileri **@articles**değişkenine atar.
- `def new` içerisindeki satır **Article** nesnesi oluşturur.
- `def create` içerisindeki satırda **article_params** kodu bir method aracılığıyla gelen verileri güvenli bir şekilde alır. Alt kısmında bulunan method'da _**kaydetme**_ işlevi başarılıysa yeni oluşturulan makalenin detaylarını gösteren sayfaya yönlendirir. Değilse makale oluşturma formunu tekrar gönderir.
- `def update` içerisindeki satır makaleyi güncellemeyi dener. Eğer işlem başarılıysa kullanıcıyı makale detaylarını göstereen sayfaya yönlendirir.
- `def destroy` içerisindeki satır makaleyi silmeyi dener. İşlem başarılıysa kayıt kalıcı olarak kaldırılır.
- `private` controller dosyasının daha iyi bir düzen içinde bulunmasını, okunabilirliğini, kod tekrarını azaltmayı ve controller dışında kullanılmasını engellemesini sağlar.
- `def article_params` Kullanıcıdan gelen verileri güvenli bir şekilde işlemek ve sadece belirli parametrelerin kabul edilmesini sağlamaktadır.
- `def set_article` Veritabanındaki bir makale kaydını belirli bir kimlik değerine göre bulup **@article** değişkenine atar. Erişebilmek için sayfanın en üst kısmında bulunan `before_action` satırı kullanılır.

![background](/assets/img/30px.png)

### CRUD Kullanımı

#### Makalelerin Gösterilmesi ve Yeni Makale Oluşturulması:

**`index.html.erb`** dosyasının içerisinde, oluşturulan makaleleri sayfamızda göstermek için;
```ruby
<% @articles.each do |article| %>
    <%= render article %>
    <p>
      <%= link_to "Makaleyi Görüntüle", article %>
    </p>
<% end %>
```
Yeni makale oluşturmak için **`<%= link_to "Yeni Makale", new_article_path %>`**

![background](/assets/img/30px.png)

#### Yeni Makalenin Oluşturulması:
_"Yeni makale"_ link'i ile gidilen konumda yeni bir makale oluşturabilmek için, **`new.html.erb`** dosyasının içerisinde; 
```ruby
<%= form_with(model: article) do |form| %>
  <div>
    <%= form.label :title%>
    <%= form.text_field :title %>
  </div>

  <div>
    <%= form.label :content%>
    <%= form.text_area :content %>
  </div>

  <div>
    <%= form.submit %>
  </div>
<% end %>
```
Makalelere geri dönmek için **`<%= link_to "Geri dön", articles_path %>`**

![background](/assets/img/30px.png)

#### Bir Makalenin İçeriği:
**`<%= link_to "Makaleyi Görüntüle", article %>`** linki ile bir makaleye gidebiliyorduk. Gittiğimiz makalenin içeriğini görüntülemek için **`show.html.erb`** dosyası içerisine;
```
<%= render @article %>
```
Makaleyi düzenleme sayfasına gitmek için **`<%= link_to "Makaleyi düzenle", edit_article_path(@article) %> |`**

Makaleyi tamamen silmek için **`<%= button_to "Makaleyi Sil", @article, method: :delete %>`**

![background](/assets/img/30px.png)

### Gem Kullanımları

Projede kullanılan gem'ler hakkında bilgi almak, kurulum ve kullanımlarını öğrenmek için;

- [Ransack](https://hamzagedikkaya.github.io/2023-08-22-gem-ransack/)
- [Image Processing](https://hamzagedikkaya.github.io/2023-08-22-gem-image/)
- [Devise](https://hamzagedikkaya.github.io/2023-08-22-gem-devise/)
- [Bootstrap](https://hamzagedikkaya.github.io/2023-08-22-gem-bootstrap/)