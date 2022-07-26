---
title: "Installation"
date: 2022-09-21T15:01:00-07:00
weight: 221
draft: false
pre: "<b>- </b>"
---

This instruction provides all you need to get OpenStudio Standard PRM installed on your local computer for scripting with OpenStudio API.

- [Download Ruby](#download-ruby)
- [Download OpenStudio](#download-openstudio)
- [Connect Ruby with OpenStudio](#connect-ruby-to-openstudio)
- [Download OpenStudio Standard](#download-openstudio-standard)

{{< line_break >}}

You will need the following applications to ensure OpenStudio Standard is running on your project.

#### Download Ruby

**Ruby 2.7**. Latest stable version is **2.7.6** and it can be downloaded at: [https://cache.ruby-lang.org/pub/ruby/2.7/ruby-2.7.6.tar.gz](https://cache.ruby-lang.org/pub/ruby/2.7/ruby-2.7.6.tar.gz)

Follow the installation guide to complete the ruby installation. You can also verify the ruby installation with the following command in the command line:

```
C:\Users\sample_user>ruby -v
ruby 2.7.4p191 (2021-07-07 revision a21a3b7d23) [x64-mingw32]
```

{{< line_break >}}

#### Download OpenStudio

**OpenStudio 3.2**. The latest stable 3.2 version is **3.2.1** and it can be downloaded at: [https://github.com/NREL/OpenStudio/releases/tag/v3.2.1](https://github.com/NREL/OpenStudio/releases/tag/v3.2.1)

More recent version of **OpenStudio** can be found in the OpenStudio release link: [https://github.com/NREL/OpenStudio/releases](https://github.com/NREL/OpenStudio/releases). The PRM is tested against **v3.4** and lower.

{{% notice tip %}}
**Importnat Note:**
The PRM routine in the OpenStudio Standard is tested against **v3.4**. However, the OpenStudio Standard is only tested on **v3.2.1** or lower.
{{% /notice %}}
{{< line_break >}}

#### Connect Ruby to OpenStudio

Once you have both Ruby and OpenStudio installed, the next thing is to connect OpenStudio with Ruby.

- **Step 1**: navigate to ruby installation directory and find the `site_ruby` folder. In Windows, it should be a directory like this:

```
C:\Ruby27-x64\lib\ruby\site_ruby
```

- **Step 2**: create a new ruby file called `openstudio.rb` under the `site_ruby` folder.
  ![Folder Image](/BEM-for-PRM/get_start/os_engine/images/connect_ruby_os_folder.PNG?width=600px&align=left&classes=border,alignLeft)

- **Step 3**: open the `openstudio.rb` file and add one line:

```ruby
require "C:/[Openstudio directory]/Ruby/openstudio.rb"
```

The directory shall point to the `openstudio.rb` file under the OpenStudio installation folder. Revise the path after `require` based on your actual installation directory.

You can verify the setting with the following command in the command line.

```
C:\Users\smaple_user> irb
irb(main):001:0> require "openstudio"
=> true
```

Return **true** verifies a success on your local setup.
{{< line_break >}}

#### Download OpenStudio Standard

Now we can create a new ruby project using the OpenStudio Standard package.
