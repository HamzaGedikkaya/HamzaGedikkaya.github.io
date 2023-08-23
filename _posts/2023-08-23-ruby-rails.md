---
layout: post
title: Ruby - Ruby on Rails
subtitle: Ruby ve Ruby on Rails Nedir? Nasıl Kurulur?
cover-img: /assets/img/rubyonrails.jpg
thumbnail-img: /assets/img/ruby.jpeg
share-img: /assets/img/rubyonrails.jpg
tags: [Ruby, Rails]
---

Dinamik, açık kaynak kodlu, basitliği ve okunabilirliği ile bilinen bir programlama dilidir. 90'lı yıllarda takma adı **"Matz"** olan Yukihiro Matsumoto tarafından oluşturulmuştur. Yazılım geliştirme alanında önemli bir prensip olan _**DRY**_ (Don't Repeat Yourself!) yani "Kendini tekrar etme!", Ruby için de geçerlidir. Matz, zarif ve güçlü bir dil oluşturmayı, geliştirici mutluluğuna ve üretkenliğine odaklanmıştır.

Öte yandan, _**Ruby on Rails**_ (genellikle Rails olarak adlandırılır.), Ruby dilinde yazılmış bir web uygulama çatısı yani bir **framework**'tür. David Heinemeier Hansson tarafından 2004 yılında Basecamp uygulamasını geliştirmek için oluşturulmuştur. Geliştiricinin verimliliğine odaklanması nedeniyle popülerlik kazanmıştır. _**Model-View-Controller**_ (MVC) mimari desenini takip eder ve varsayılan olarak veritabanı, web servisi ve web sayfaları için yapısal yapılara sahiptir. Kullanım kolaylığı ve hız sağlamak amacıyla web geliştirme süreçlerini basitleştirmeyi hedefler.

![background](/assets/img/30px.png)



## Homebrew Kurulumu    

Homebrew, macOS ve Linux işletim sistemlerinde kullanılan paket yöneticisi ve paket oluşturma aracıdır. Kullanıcıların kolay bir şekilde paket _**yüklemeleri**_, güncellemeleri ve kaldırmalarını sağlar.

>Terminale şu komutu girerek homebrew'i kurabilirsiniz:

```python
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

![background](/assets/img/30px.png)
    


## Ruby Kurulumu  


1. Ruby programlama dilini **_ASDF_** adlı bir sürüm yöneticisi kullanarak kuracağız. ASDF'yi rbenv, rvm veya diğer araçların yerine kullanmamızın sebebi _Node.js_ gibi diğer dilleri de yönetebilmesidir.

    ```bash
    cd
    git clone https://github.com/excid3/asdf.git ~/.asdf
    echo '. "$HOME/.asdf/asdf.sh"' >> ~/.zshrc
    echo '. "$HOME/.asdf/completions/asdf.bash"' >> ~/.zshrc
    echo 'legacy_version_file = yes' >> ~/.asdfrc
    exec $SHELL
    ```

    ![background](/assets/img/30px.png)

2. ASDF eklentilerini kurmak için aşağıdaki komutları kullanabiliriz; _Rails için Ruby, JS için Node.js kurulabilir._

    ```bash
    asdf plugin add ruby
    asdf plugin add nodejs
    ```
    ![background](/assets/img/30px.png)

3. Ruby'yi yüklemek için aşağıdaki komutları gireceğiz. Fakat içeriğin güncel olmaması durumuna karşı [kendi sitesinden](https://www.ruby-lang.org/en/downloads/) son sürümünün bu olduğuna emin olun. 

    ```javascript
    asdf install ruby 3.2.2
    asdf global ruby 3.2.2

    # Update to the latest Rubygems version
    gem update --system
    ```
    ![background](/assets/img/30px.png)

4. Yüklenilen sürümü görüntülemek ve yüklendiğinden emin olmak için **-version** komutunu kullanabiliriz.

    ```javascript
    which ruby
    #=> /Users/username/.asdf/shims/ruby
    ruby -v
    #=> 3.2.2
    ```
    ![background](/assets/img/30px.png)

5. _Opsiyonel olarak_ Rails uygulamalarımızda JS'i terminalde çalıştırabilmek için Node.js'i yükleyebiliriz.

    ```python
    asdf install nodejs 18.16.1
    asdf global nodejs 18.16.1

    which node
    #=> /Users/username/.asdf/shims/node
    node -v
    #=> 18.16.1

    # Install yarn for Rails jsbundling/cssbundling or webpacker
    npm install -g yarn
    ```
    ![background](/assets/img/30px.png)



## Rails Kurulumu

1. Rails kurulumu için [güncel versiyonu kontrol ettikten sonra](https://rubyonrails.org/) aşağıdaki komutu çalıştırabilirsiniz

    ```javascript
    gem install rails -v 7.0.6
    ```
    ![background](/assets/img/30px.png)


2. Kurulum tamamlandıktan sonra doğrulamak için;

     ```javascript
    rails -v
    # Rails 7.0.6
    ```
    ![background](/assets/img/30px.png)



## Veritabanı Kurulumu

Rails projelerimizde kullanacağımız veritabınını kurmak için birden fazla seçeneğimiz vardır. Herhangi biri tercih edilebilir;

```bash
#SQLİTE3 
brew install sqlite3

#MYSQL
brew install mysql

#POSTGRESQL
brew install postgresql
```
![background](/assets/img/30px.png)