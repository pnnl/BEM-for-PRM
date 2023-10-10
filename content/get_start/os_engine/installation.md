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

- Download **Ruby 2.7.x**. Latest stable version is **2.7.2** and it can be downloaded at [https://www.ruby-lang.org/en/news/2020/10/02/ruby-2-7-2-released/](https://www.ruby-lang.org/en/news/2020/10/02/ruby-2-7-2-released/).

- You can also verify the ruby installation with the following command in the command line:
  ```
  C:\Users\sample_user>ruby -v
  ruby 2.7.2p137 (2020-10-01 revision 5445e04352) [x64-mingw32]
  ```

{{< line_break >}}

##### **2. Download OpenStudio**

- Download **OpenStudio 3.2.x**. The latest stable version is **3.2.1** and it can be downloaded at: [https://github.com/NREL/OpenStudio/releases/tag/v3.2.1](https://github.com/NREL/OpenStudio/releases/tag/v3.2.1)

- More recent version of **OpenStudio** can be found in the OpenStudio release link: [https://github.com/NREL/OpenStudio/releases](https://github.com/NREL/OpenStudio/releases). The PRM is tested against **v3.4** and lower.

  {{% notice tip %}}
  **Important Note:**
  The PRM routine in the OpenStudio Standard is tested against **v3.4**. However, the OpenStudio Standard is only tested on **v3.2.1** or lower.
  {{% /notice %}}

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
