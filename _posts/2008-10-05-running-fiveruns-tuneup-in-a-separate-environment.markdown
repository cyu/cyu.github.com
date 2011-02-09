--- 
wordpress_id: 168
layout: post
title: Running FiveRuns TuneUp in a Separate Environment
wordpress_url: http://blog2.codeeg.com/?p=168
---
<a href="http://www.fiveruns.com/products/tuneup">FiveRuns' TuneUp</a> is a great tool for profiling your Rails app, but by default it is always running in development.  This causes two issues 1) every request is slower in development as it is always collecting profiling data, and 2) the TuneUp bar can mess with the layout of your application, especially if you're rendering content in iframes like we do with <a href="http://skribit.com">Skribit</a>.  So instead of having TuneUp always run in development mode, I've changed it to run only when the server is started in a new environment called 'profiler'.   Here's how I did it.

First, you need to tell TuneUp to only run in the profiler environment.  You do this by modifying <em>config/tuneup.rb</em> (create this file if it doesn't already exist):
<pre><code>Fiveruns::Tuneup.config do |config|
  config.environments.delete('development')
  config.environments &lt;&lt; 'profiler'
end</code></pre>

Next, copy the development configuration block in <em>config/database.yml</em> and create a new block called <em>profiler</em>:
<pre><code>profiler:
  adapter: mysql
  encoding: utf8
  database: YOUR_DATABASE
  username: YOUR_USERNAME
  password: YOUR_PASSWORD
  socket: /tmp/mysql.sock
</code></pre>

Finally, create your profiler environment configs by copying your development configs:
<pre><code>cp config/environments/development.rb config/environments/profiler.rb</code></pre>

You're all set!  Now, to run the server with TuneUp on, just run the server in the <em>profiler</em> environment:
<pre><code>script/server -e profiler</code</pre>

Now, I have TuneUp available to me only when I'm looking to optimize performance.
