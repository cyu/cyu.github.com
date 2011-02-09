--- 
wordpress_id: 145
layout: post
title: Problems With Rails Fixture Labels?
wordpress_url: http://dontforgettoplantit.wordpress.com/?p=149
---
Newer versions of Rails has a nice feature where you can use label references for fixtures.  So instead of:
<pre><code># posts.yml
test_post:
  user_id: 1
  title: My Test Post
</code></pre>

You can do this:
<pre><code>test_post:
  user: quentin
  title: My Test Post
</code></pre>

However, if your model class name is in a pluralized form, you might find that label references won't work.  That's because fixtures derive their class name from the singular form of the table name by default.  Fortunately, you can fix this by adding this line to your TestHelper:
<pre><code>class Test::Unit::TestCase

  # Explicitly map the table name to class name
  set_fixture_class :accounts =&gt; 'accounts'
end
</code></pre>

Hopefully, this will save someone else from having to dig through the Rails fixtures internals.
