---
layout: post
title: How to change the hash_secret of Paperclip
tags:
- paperclip
- rails
- migration
image:
  feature: abstract-1.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
---

Recently, In Paperclip (4.1.1) I had to change the `hash_secret`, which is used for the purpose of [URI Obfuscation](https://github.com/thoughtbot/paperclip#uri-obfuscation). But, the problem was that there had been many files in production generated with the old `hash_secret`. Simply just changing the `hash_secret` would invalidate all the old URL's as paperclip does not store the URL's of the files instead it generates using the `path` option given to it (which would obviously contain the `:hash` interpolation).

It was evident that I had to write a migration to move the already existing files from old location to new location (We were storing the files in S3). Also, I wanted the moving part to simply look like 

{% highlight ruby %}
old_object = bucket.objects[attachment.file.old_path]
new_object = bucket.objects[attachment.file.path]

old_object.move_to(new_object)
{% endhighlight %}

In order for `file.old_path` to work I had to override `Paperclip::Attachment` and add `old_path` method to it. Although, it turned out that I had to override three more functions, it was worth it. Besides, this override was just a temporary one.

{% highlight ruby %}
# config/initializers/paperclip_override.rb

require 'openssl'

module Paperclip
  class Attachment

    # Old path overrides
    def old_path(style_name = default_style)
      path = original_filename.nil? ? nil : interpolate(old_path_option, style_name)
      path.respond_to?(:unescape) ? path.unescape : path
    end

    def old_path_option
      # Replace this with your :path option replacing :hash with :old_hash
      "file_attachments/:id/:old_hash.:extension"
    end

    def old_hash_key(style_name = default_style)
      data = interpolate(@options[:hash_data], style_name)
      OpenSSL::HMAC.hexdigest(OpenSSL::Digest.const_get(@options[:hash_digest]).new,
        "<YOUR OLD HASH_SECRET>", data)
    end
  end

  module Interpolations
    def old_hash attachment=nil, style_name=nil
      if attachment && style_name
        attachment.old_hash_key(style_name)
      else
        super()
      end
    end
  end
end
{% endhighlight %}

Here is the rake task which moves the file from old location to the new location just as I wanted it to be,

{% highlight ruby %}
# lib/tasks/paperclip_hash_secret_migration.rake

namespace :paperclip do
  task :s3_migration => :environment do
    Attachment.all.each do |attachment|
      s3 = AWS::S3.new(AWS_S3["credentials"])
      bucket = s3.buckets[AWS_S3["bucket_name"]]

      old_object = bucket.objects[attachment.file.old_path]
      new_object = bucket.objects[attachment.file.path]

      puts "Moving object from #{attachment.file.old_path} to #{attachment.file.path}"
      old_object.move_to(new_object, { :acl => :public_read })

      # Perform any additional operations you want with
      # old_object and new_object here.
    end
  end
end
{% endhighlight %}

If you are using a File Storage, then you could find appropriate ruby code to move file from one path to another on [Google](https://www.google.co.in/webhp?#safe=off&q=ruby+move+file+to+new+location).
