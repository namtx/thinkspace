---
layout: post
label: til
title: "Hash deep compact"
---

<p>
  
</p>
```ruby
class Hash
  def deep_compact
    delete_if do |_k, v|
      case v
      when Hash
        v.deep_compact.empty?
      when Array
        v.empty?
      else
        v.nil?
      end
    end
  end
end
```
