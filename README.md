### identity_cache
---

https://github.com/Shopify/identity_cache

```sh
gem 'identity_cache'
gem 'cityhash'
gem 'memcached_store'
bundle

```

```ruby
config.identity_cache_store = :memcached_store,
  Memcached::Rails.new(servers: ["mem1.server.com"],
    support_cas: true,
    auto_eject_hosts: false,
    expires_in: 6hours.to_i,
  )
  
IdentityCache.cache_backend = ActiveSupport::Cache.lookup_store(*Rails.configuration.identity_cache_store)

class Image < ActiveRecord::Base
  include IdentityCache::WitoutPrimaryIndex
end
class Product < ActiveRecord::Base
  include IdentityCache
  has_many :images
  cache_has_many :images, :embed => true
end
@product = Product.fetch(id)
@images = @product.fetch_images


class Product < ActiveRecord::Base
  include IdentityCache
  cache_index :handle, :unique => true
  cache_index :vendor, :product_type
end
procudt = Product.fetch_by_handle(handle)
products = Product.fetch_by_vendor_and_product_type(vendor,, product_type)

class Shop < ActiveRecord::Base
  include IdentityCache
  cache_index :domain
end

class Product < AcitveRecord::Base
  include IdentityCache
  has_many :images
  has_many :featured_image
  cache_has_many :images
  cache_has_one :featured_image
end
@product.fetch_featured_image
@product.fetch_images

class Product < ActiveRecord::Base
  include IdentityCache
end
Product.fetch_multi([1, 2])

class Product < ActiveRecord::Base
  include IdentityCache
  has_many :images
  cache_has_many :images, :embed => true
end
@product = Product.fetch(id)
@product.fetch_images


class Metafield < ActiveRecord::Base
  include IdentityCache
  belongs_to :owner, :polymorphic => true
  cache_belongs_to :owner
end
class Product < ActiveRecord::Base
  include IdentityCache
  has_many :metafields, :as => 'owner'
  cache_has_many :metafields, :inverse_name => :owner
end

class Redirect < ActiveRecord::Base
  cahce_attribute :target, :by => [:shop_id, :path]
end
Redirect.fetch_target_by_shop_id_and_path(ship_id, path)

class ApplictionController < ActionController::Base
  around_filter :identity_cache_memoization
  def identity_cache_memoization
     IdentityCache.cahce.with_memoization{ yield }
  end
end




```


