---
layout: post
disqus: true
title:  "Download files with Ruby from a server"
date:   2018-08-03 19:20:00 -0300
categories: ruby
---

A fews months ago a client ask me to make a script for a Rails app, this script would be responsible for uploading multiple files of different pages to a Google account. At first, I didn't think it would be a complicated task, because I would use Capybara to interact with the pages and download the files and Capybara is easy to use. I'm going to give an example of how it would be to download a file with Capybara.

First we would have to configure our enviroment.

- `gem install 'selenium-webdriver'` install selenium, allows us to interact with the browser
- `gem install 'capybara'` install capybara(this uses selenium)
- `gem install 'geckodriver-helper'` install geckodriver to use firefox (also you can use chrome but you need a different gem)

In the next step we need configure Capybara.

```
# capybara_example.rb
# Call capybara gem
require 'rubygems'
require 'capybara'
require 'capybara/dsl'
require 'selenium-webdriver'

# Stop capybara
Capybara.run_server = false

# We registered a driver (in this case I named it selenium)
Capybara.register_driver :selenium do |app|
  # Since I'm going to use Firefox, I first set up some options, such as downloading the files, forcing the download
  options = Selenium::WebDriver::Firefox::Options.new
  options.add_preference('browser.download.dir', '~/')
  options.add_preference('browser.download.folderList', 2)
  options.add_preference('browser.helperApps.alwaysAsk.force', false)
  options.add_preference('browser.download.manager.showWhenStarting', false)
  options.add_preference('browser.helperApps.neverAsk.saveToDisk', 'text/csv')
  options.add_preference('csvjs.disabled', true)

  # In Capybara add a driver
  Capybara::Selenium::Driver.new(app, browser: :firefox , options: options)
end
# I set it by default
Capybara.current_driver = :selenium
```

There are different drivers that you can use; PhantomJs, Chrome, Firefox (the first one doesn't need a graphical environment and can run without problems on servers, usually you can use it to do integration tests)

The second thing is to download a file with Capybara

```
# You could do the following, visit the page, click on a button, wait 5 seconds to start downloading the file

Capybara.visit('https://www.sample-videos.com/download-sample-csv.php')
Capybara.first('.download_csv').click
sleep(5)

```

The complete file would be as follows:

```
# capybara_example.rb
require 'rubygems'
require 'capybara'
require 'capybara/dsl'
require 'selenium-webdriver'

Capybara.run_server = false
Capybara.register_driver :selenium do |app|
  options = Selenium::WebDriver::Firefox::Options.new

  options.add_preference('browser.download.dir', '~/')
  options.add_preference('browser.download.folderList', 2)
  options.add_preference('browser.helperApps.alwaysAsk.force', false)
  options.add_preference('browser.download.manager.showWhenStarting', false)
  options.add_preference('browser.helperApps.neverAsk.saveToDisk', 'text/csv')
  options.add_preference('csvjs.disabled', true)

  Capybara::Selenium::Driver.new(app, browser: :firefox , options: options)
end

Capybara.current_driver = :selenium

Capybara.visit('https://www.sample-videos.com/download-sample-csv.php')
Capybara.first('.download_csv').click
sleep(5)
```

If you run this file it should work. With Capybara interacting with different pages is very simple, you need to press buttons and fill out forms, just that.

This script works on any machine, what happens is that this script should be executed on a SERVER, where we don't have an interface to run the browser, we could use PhantomJs or use the headless of each browser but this isn't possible download files....so, what do we do? That's where XVFB appears, which is Virtual framebuffers to run the graphical part without having one (__NOTE! __: this only works for Linux, in Mac it isn't yet)

You would have to do the following:

- `sudo apt-get install xvfb` install virtual framebuffer
- `gem install headless` this use XVFB gem

And then we use it in our code!

```
require 'rubygems'
require 'capybara'
require 'capybara/dsl'
require 'selenium-webdriver'
require 'headless'


headless = Headless.new
headless.start
 
Capybara.run_server = false
Capybara.register_driver :selenium do |app|
  options = Selenium::WebDriver::Firefox::Options.new

  options.add_preference('browser.download.dir', '~/')
  options.add_preference('browser.download.folderList', 2)
  options.add_preference('browser.helperApps.alwaysAsk.force', false)
  options.add_preference('browser.download.manager.showWhenStarting', false)
  options.add_preference('browser.helperApps.neverAsk.saveToDisk', 'text/csv')
  options.add_preference('csvjs.disabled', true)

  Capybara::Selenium::Driver.new(app, browser: :firefox , options: options)
end

Capybara.current_driver = :selenium

Capybara.visit('https://www.sample-videos.com/download-sample-csv.php')
Capybara.first('.download_csv').click
sleep(5)

headless.destroy
```


With this we could run it on any Linux server

Any questions, just comment below!