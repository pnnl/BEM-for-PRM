---
title: "Installation"
date: 2022-09-21T15:01:00-07:00
weight: 231
draft: false
pre: "<b>- </b>"
---

{{<line_break>}}

#### A. Steps to install and get the OpenStudio PRM running for scripting with OpenStudio API

There are 4 parts to this section.

1. [**Download Ruby**](#1-download-ruby)
2. [**Download OpenStudio**](#2-download-openstudio)
3. [**Connect Ruby with OpenStudio**](#3-connect-ruby-to-openstudio)
4. [**Download OpenStudio Standard**](#4-download-openstudio-standard)

{{< line_break >}}

<!-- You will need the following applications to ensure OpenStudio Standard is running on your project.-->

##### **1. Download Ruby**

**Ruby 2.7**. Compatible version is **2.7.2** and it can be downloaded at: [https://cache.ruby-lang.org/pub/ruby/2.7/ruby-2.7.2.tar.gz](https://cache.ruby-lang.org/pub/ruby/2.7/ruby-2.7.2.tar.gz)

- You can verify the ruby installation with the following command in the command line:
  ```
  C:\Users\sample_user>ruby -v
  ruby 2.7.2p137 (2020-10-01 revision 5445e04352) [x64-mingw32]
  ```

{{< line_break >}}

##### **2. Download OpenStudio**

**OpenStudio 3.7**. The latest stable 3.7 version is **3.7.0** and it can be downloaded at: [https://github.com/NREL/OpenStudio/releases/tag/v3.7.0](https://github.com/NREL/OpenStudio/releases/tag/v3.7.0)
More recent version of **OpenStudio** can be found in the OpenStudio release link: [https://github.com/NREL/OpenStudio/releases](https://github.com/NREL/OpenStudio/releases). The PRM is tested against **v3.7** and lower.

{{< line_break >}}

##### **3. Connect Ruby to OpenStudio**

Once you have both Ruby and OpenStudio installed, the next thing is to connect OpenStudio with Ruby.

- **Step 1**: Navigate to ruby installation directory and find the `site_ruby` folder. In Windows, it should be a directory like this:

  ```
   C:\Ruby27-x64\lib\ruby\site_ruby
  ```

- **Step 2**: Create a new ruby file called `openstudio.rb` under the `site_ruby` folder.
  ![Folder Image](/BEM-for-PRM/get_start/os_engine/images/connect_ruby_os_folder.PNG?width=600px&align=left&classes=border,alignLeft)

- **Step 3**: Open the `openstudio.rb` file and add one line:

  ```ruby
  require "C:/[Openstudio directory]/Ruby/openstudio.rb"
  ```

- The directory shall point to the `openstudio.rb` file under the OpenStudio installation folder. Revise the path after `require` based on your actual installation directory.

- You can verify the setting with the following command in the command line.

  ```
  C:\Users\sample_user> irb
  irb(main):001:0> require "openstudio"
  => true
  ```

  Return **true** verifies a success on your local setup.

{{< line_break >}}

##### **4. Download OpenStudio Standards**

- Install the gem **_OpenStudio Standards_**.
  Open command prompt and type the following.
  ```ruby
  gem install openstudio-standards
  ```
  {{<line_break>}}

##### **Now we can create a new ruby project using the OpenStudio Standard package.**

The OpenStudio Standard package is hosted on Github as an open-source project. The latest PRM method development is in the [Appendix_G](https://github.com/NREL/openstudio-standards/tree/AppendixG_Dev) branch. You can either clone the branch or download it as a zip package. To run the package, import the path to your ruby script as the example shown below.

```ruby
require_relative 'C:\\Users\\Documents\\OSSTD_Repo\\lib\\openstudio-stadnards.rb'
```
