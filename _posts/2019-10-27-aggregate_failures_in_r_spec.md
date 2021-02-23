---
layout: post
label: til
title: "aggregate_failures in RSpec"
---

<p>
  
  	<span class="issue-label" style="background-color: e83c53">rspec</span>
  
  	<span class="issue-label" style="background-color: b517d1">ruby</span>
  
</p>
`aggregate_failues` is an API of `RSpec::Expectations`, allows you to group a set of all failures at once, rather than its aborting when see first failure.

```ruby
describe '.from_string', :aggregate_failures do
  it 'something failure' do
  end

  it 'also failure too' do
  end
end
```

