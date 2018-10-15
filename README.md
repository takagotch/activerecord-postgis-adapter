### activerecord-postgis-adapter
---

https://github.com/rgeo/activerecord-postgis-adapter

 ```
 gem 'activerecord-postgis-adapter'
 gem 'activerecord-postgis-adapter'
 gem 'activerecord-jbcpostgresql-adapter', '~> 1.3.9'
 gem 'ffi-geos'
 
 gem 'activerecord-postgis-adapter', '~> 0.6.6'
 
 rails new my_app --database=postgresql
 
 gem 'acitverecord-postgis-adapter'
 rake db:create
 
 rake db:gis:setup
 
 SELECT POSTGIS_VERSION();
 ```

```ruby
create_table :my_spatial_table do |t|
  t.column :shape1, :geometry
  t.geometry :shape2
  t.line_stirng :path, srid: 3785
  t.st_point :lonlat, geographic: true
  t.st_point :lonlaheight, geographic: true, has_z: true
end

add_index :my_table, :lonlat, using: :gist

change_table :my_table do |t|
  t.index :lonlat, using: :gist
end

RGeo::ActiveRecord::SpatialFactoryStore.instance.tap do |config|
  config.default = RGeo::Geos.factory_generator
  config.register(RGeo::Geographic.spherical_factory(srid: 4326), geo_type: "point")
end

record = MySpatialTable.find(1)
p = record.lonlat
puts p.x
puts p.geometry_type.type_name

factory = p.factory

record.lonlat = 'POINT(-122 47)'
record.lonlat = 'POINT(x)'
p2 = factory.point(-122, 47)
record.lonlat = p2
record.shape2 = p2
record.save

record.path = p2
record.save

record2 = MySpatialTable.where(:lonlat => factory.point(-122, 47)).first
record3 = MySpatialTable.where(:lonlat => 'POINT(-122 47)').first


```

```
development:
  username: tky
  adapter: postgis
  host: localhost
  schema_seach_path: public
  
development: 
  schema_search_path: public, postgis
  
development:
  adapter: postgis
  encoding: unicode
  postgis_extension: postgis
  postgis_schema: public
  schma_seach_path: public,postgis
  pool: 5
  database: my_app_development
  username: my_app_user
  password: my_app_password
  su_username: my_global_user
  su_password: my_global_password
```
