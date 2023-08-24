---
layout: post
title: Gem Bootstrap
subtitle: Bootstrap Nedir? Nasıl Kullanılır?
cover-img: /assets/img/bootstrap.jpeg
thumbnail-img: /assets/img/gem.jpeg
# share-img: /assets/img/bootstrap.jpeg
tags: [Ruby, Rails, GEM, Bootstrap]
---

Bootstrap, front-end framework olarak sıklıkla kullanılan ***CSS*** kütüphanesidir. Bootstrap'i projemize eklememiz için iki yol vardır. 

- Birinci yol CDN'den (Content Delivery Network) yani bir link aracılığı ile, uzaktan projeye sunulur.
- İkinci yol yerel olarak projemize dahil etmektir ki bu kuracağımız gem ile sağlanabilir.
>- CDN yerine yerel olarak kurmamızın sebeplerini yükleme ve hız, sürdürülebilirlik, güncellemeler, gizlilik ve güvenlik başlıkları altında toplayabiliriz.

### Bootstrap Nasıl Kurulur?

1. Gayet basit bir kuruluma sahip olan bu gem'i kurmak için ***`gemfile`*** dosyamıza şu iki satırı eklemeliyiz:

    ```ruby
    gem "bootstrap"
    gem "sassc-rails"
    ```
    Ardından terminalde ***`bundle install`*** komutunu çalıştırmalıyız.

2. ***app/assets/stylesheets/application.scss*** (uzantı .css ise .scss'e çevirmeliyiz.) konumuna gidip dosya içerisine ***`@import "bootstrap";`*** satırını eklememiz bootstrap'i kurmak için yeterli olacaktır.