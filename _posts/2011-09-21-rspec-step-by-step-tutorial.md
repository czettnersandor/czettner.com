---
layout: post
title: RSpec step-by-step tutorial
created: 1316626924
comments: true
categories: [ruby]
---
My last <a href="http://sandor.czettner.hu/blog/11/06/24/phpunit-tesztek-minden-elott">PHP tutorial</a> was about PHPUnit tests. Now let's learn about the more elegant Ruby BDD/TDD framework, named rspec.

I'm using a newly installed Archlinux. First, install <a href="https://github.com/sstephenson/rbenv">sstephenson</a>'s Ruby Version Management software:
{% highlight html %}
$ yaourt rbenv
...
$ yaourt ruby-build-git
...
{% endhighlight %}
Now, build ruby the proper way:
{% highlight html %}
$ rbenv install 1.9.2-p290
...
$ ruby -v
ruby 1.9.2p290 (2011-07-09 revision 32553) [x86_64-linux]
{% endhighlight %}
Install bundler, the best way to define GEM requirements in project level:
{% highlight html %}
$ sudo gem install bundler
{% endhighlight %}
Create the project directory and some initial files:
{% highlight html %}
$ mkdir rspec-tutorial
$ mkdir test
$ mkdir lib
$ touch Rakefile
$ touch Gemfile
{% endhighlight %}
Edit Gemfile:
{% highlight ruby %}
source "http://rubygems.org"
gem "rspec", "1.3.1"
gem "activesupport", "3.0.5"
{% endhighlight %}
And Rakefile:
{% highlight html %}
require 'rake'
require 'spec/rake/spectask'

desc "Run all examples"
Spec::Rake::SpecTask.new('test') do |t|
  t.spec_files = FileList['test/**/*.rb']
end
{% endhighlight %}
Install all required gems by typing: "bundle install". Now, if you put "rake test" in command line, the test will run with no output (there are no test cases). Let's create some application logic. I will re-implement my PHP Log class <a href="http://sandor.czettner.hu/blog/11/06/24/phpunit-tesztek-minden-elott">described before</a>. Put this to test/log_spec.rb:
{% highlight ruby %}
require './lib/log.rb'
LOGFILEPATH = '/tmp/loggertest.log'

describe Log do
  before(:all) do
    @log = Log.new(LOGFILEPATH)
  end
  after(:all) do
    File.delete(LOGFILEPATH)
  end
  
  it "should exist after instantiation" do
    File.exist?(LOGFILEPATH).should be_true
  end
  
  it "shouldn't be overwrittean after re-instantiation" do
    @log.log_write "Hello World"
    @log.close
    @log = nil
    @log = Log.new(LOGFILEPATH)
    File.size(LOGFILEPATH).should > 0
  end
end
{% endhighlight %}
And to lib/log.rb:
{% highlight ruby %}
class Log
  def initialize(filename)
    @file = File.open(filename, "a")
  end
  
  def log_write(text)
    @file.write(text+"\n")
  end
  
  def close
    @file.close
  end
end
{% endhighlight %}

As you can see, the rspec test examples have a very readable syntax:
{% highlight ruby %}
  it "should exist after instantiation" do
    File.exist?(LOGFILEPATH).should be_true
  end
{% endhighlight %}

But what is before(:all) and after(:all) does mean? Everything in the do-end block will execute once. Before starting testcase and after all test examples done in the file. You can also use the ":each" when you want to execute anything every time a test example run.

Some other assertions:
{% highlight ruby %}
  'foo'.should == 'foo'
  'foo'.should === 'foo'
  'foo'.should_not equal('foo')
  ''.should be_empty
  'foo with bar'.should include('with')
  'http://fr.ivolo.us'.should match(/http:\/\/.+/i)
  nil.should be_nil
  100.should < 200
  200.should >= 100
  (200 - 100).should == 100
  # (100 - 80) is less than 21
  100.should be_close(80,21)
  [1,2,3].should have(3).items
  [].should be_empty
  [1,2,3].should include(2)
  {}.should be_empty
  {:post => {:title => 'test'}}.should have_key(:post)
  {:post => {:title => 'test'}}.should_not have_key(:title)
  false.should be_false
  true.should be_true
  @post.should be_instance_of(Post)
  @post.should respond_to(:title)
{% endhighlight %}

Feel free to try everything you want and define more "it must be work, should be true" sentences, run them and create bigger applications with team-players like you. Next, we will see "Cucumber".

Whole source code available here:

https://github.com/czettnersandor/Rspec-Log-testing-tutorial
