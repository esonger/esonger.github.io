---
layout: post
title: python国际化和本地化
categories: python Babel
description: 国际化 本地化
keywords: python Babel
---
Babel官方文档：<http://babel.pocoo.org/en/latest/>

## Babel 介绍
Babel 是一个完整的使用工具集合，主要帮助基于web的python应用程序实现国际化和本地化。
Babel提供给国际化(I18n)和本地化(L10N)的功能被分成两个不同的方面：   
* tools，构建和使用消息列表
* interface，本地数据仓库的python接口，支持存取多样的本地名称、数字和日期格式等。

### Message catalogs
{{site.indent}}虽然python标准库包含了可以是应用成使用message catalogs的gettext模块，但是它要求开发者使用GNU工具
如xgettext，msgmerge和msgfmt来构建message catalogs。xgettext虽然支持从python文件中提取messages，
但是它不知道如何从python web-application中其他类型的文件提取，如模板文件，而且也没有提供一个简单的扩展机制来增加这种提取。
Babel提供一个框架，包括多种提取方法，并且移除了GNU gettext工具。

### Local data
{{site.indent}}此外，虽然python标准库包含了对localization的支持，但是基于每个process仅有一个特定的locale被使用的假想。
另外，它不支持存取其他类型数据， 如国家的本地化的名称，语言或时区，这些在web-based applacations中经常需要。
{{site.indent}}基于这些要求，Babel包含了从Common Locale Data Repository (CLDR)中提取的数据，
并提供一些方便的方法来存取和使用这些数据。

## install

~~~
pip install Babel
~~~

## Local data
{{site.indent}}message catalogs 允许本地化你应用中的任何messages。有一些字符串被应用到许多应用中，是可以容易获取的。
{{site.indent}}举个例子，想象有个用户可选择的国家列表，你想以用户的语言显示这些国家的名称。你可以Babel种包含的locale data提供的翻译
而不是在你的应用中自己翻译所有的国家名称。这些翻译基于Common Locale Data Repository (CLDR)，由Unicode Consortium开发和维护。

### The Locale Class
通常通过Locale class存取locale data:
~~~ python
>>> from babel import Locale
>>> locale = Locale('en', 'US')
>>> locale.territories['US']
u'United States'
>>> locale = Locale('es', 'MX')
>>> locale.territories['US']
u'Estados Unidos'
~~~
{{site.indent}}除了国家/区域名称，locale data 还可以提供语言名，脚本，变体，市区等等。有些数据与数字和日期格式是紧密相关的。
大部分locale 相应的属性返回字典，用一个code作为key，比如ISO国家和语言代码。

### Likely Subtags
{{site.indent}}当你处理locale时可能遇到locale tag 没有完全描述的情况。比如，人们常提到的zh_TW，这个标识符不能被解析成一个locale。
Babel的标识符解析程序尝试去解决，并以一个intended locale结束:   
~~~ python
>>> from babel import Locale
>>> Locale.parse('zh_TW')
Locale('zh', territory='TW', script='Hant')
~~~

### Locale Display Name
区域自己可以被用来描述自己或其他区域。主要的意思是说，对已给的local对象，你可以问它标准的显示名称，语言名称或其他。
由于locales互相交叉引用，你可以用热河CLDR支持的语言描述区域的名称。 
~~~ python
>>> l = Locale.parse('de_DE')
>>> l.get_display_name('en_US')
u'German (Germany)'
>>> l.get_display_name('fr_FR')
u'allemand (Allemagne)'
~~~
显示名称包含了辨别一个区域的所有信息，你可以也会需要部分信息:
~~~ python
>>> l.get_language_name('de_DE')
u'Deutsch'
>>> l.get_language_name('it_IT')
u'tedesco'
>>> l.get_territory_name('it_IT')
u'Germania'
>>> l.get_territory_name('pt_PT')
u'Alemanha'
~~~

### Canlendar Display Names
Locale class 提供了存取与日历显示相关显示名称，如周的名称或月的名称
~~~
>>> locale = Locale('es')
>>> month_names = locale.months['format']['wide'].items()
>>> for idx, name in sorted(month_names):
...     print name
enero
febrero
marzo
abril
mayo
junio
julio
agosto
septiembre
octubre
noviembre
diciembre
~~~

## Date and Time
<http://babel.pocoo.org/en/latest/dates.html>

## Number Formatting
<http://babel.pocoo.org/en/latest/numbers.html>

## Working with Message Catalogs
### Introduction
gettext翻译系统使你能标记在应用中使用的任何字符串来本地化,使用 `gettext(str)` 和 `ngettext(singular, plural, num)` 。
为了简洁`gettext`方法经常重命名为`_(str)`：  
用
~~~
print _("Hello")
~~~
代替
~~~
print "Hello"
~~~
来标记     
{{site.indent}}Message catalogs 是一个在应用中可本地化信息的翻译的集合。
她们通常存储在PO(Portable Object) and MO(Machine Object)文件中，其格式由GNU gettext tools和translation project定义。   
一般生成Message catalogs的流程:    
* 使用工具(如`xgettext`)从代码中提取可本地化的字符串并在写到POT(PO Template)文件。
* 复制一份POT文件到对应地区，就可以开始翻译了
* 使用工具(如`msgfmt`)把PO文件编译成MO文件
* 当代码更新，需重新生成POT文件然后合并到对应地区的PO文件，如使用`msgfmt`    

Python提供了`gettext`模块座位标准库的一部分,是应用程序可以和生成的MO文件很好的工作。
由于`gettext`提供了翻译应用程序中的信息，Babel没有重新发明轮子而是使用这个基础设施，并且使它更容易生成`message catalogs`。

### Message Extraction
Babel提供一个类似`xgettext`的程序，不仅能从python源文件还可以从其他文件格式的文件提取。
`xgettext`，通常要每个文件调用一次，而Babel的message提取操作是在目录之上的。
虽然每个文件执行`xgettext`对使用`Makefile`的工程很好，但python工程很少使用make，因此，需要一个不同的从多种文件提取messages的机制。
message提取是基于目录而不是单个文件，因此需要有一种方式来配置哪些文件需要被处理。    

### Extraction Method Mapping and Configuration
在Babel中，对应文件和提取方法的映射关系是通过配置文件来实现的。配置文件把模式映射到提取方法的名称，并且可为每个模式设置多个选项
如下配置在text模板和Genshi markup模板添加消息提取:
~~~
# Extraction from Python source files

[python: **.py]

# Extraction from Genshi HTML and text templates

[genshi: **/templates/**.html]
ignore_tags = script,style
include_attrs = alt title summary

[genshi: **/templates/**.txt]
template_class = genshi.template:TextTemplate
encoding = ISO-8819-15

# Extraction from JavaScript files

[javascript: **.js]
extract_messages = $._, jQuery._
~~~
配置文件的语法基于.INI格式，在python中`Configparser`支持这种格式。
Section 名称用冒号分隔指定了提取方法的名称和the extended glob pattern，在sections里的可选项传递给提取方法。
一个通配符*表示匹配任意多个字符，?表示匹配一个字符。另外两个**在任意目录下的文件，如**.txt,表示匹配任意目录下以.txt结尾的文件。
`#`或`;`字符表示注释，会被忽略，空行也将被忽略。

#### Default Extraction Methods
Babel有一些内置提取器：  
1. python：从python源文件中提取message
2. javascript：从JavaScript文件提取
3. ignore：忽略提取message

python提取器默认匹配`**.py`模式。加入你知道自定义的映射配置，默认的映射则会失效。

#### Referencing Extraction Methods
为了使短的提取方法名称(如：genshi)可用，你需要安装`pkg_resources`，并且要带它的meta data一起安装。
如果因为某些不能原因，你需要在`[extractors]`中把短名称映射到函数名的完全描述。例如：
~~~
# Some custom extraction method

[extractors]
custom = mypackage.module:extract_custom

[custom: **.ctm]
some_option = foo
~~~
内置提取方法python和ignore默认是可获取的，即使`pkg_resources`没有安装，也不必再`[extractors]`中明确定义。

### Writing Extraction Methods
添加一个新的提取方法是容易的。首先，你需要按下面接口实现一个方法：
~~~ python
def extract_xxx(fileobj, keywords, comment_tags, options):
    """Extract messages from XXX files.

    :param fileobj: the file-like object the messages should be extracted
                    from
    :param keywords: a list of keywords (i.e. function names) that should
                     be recognized as translation functions
    :param comment_tags: a list of translator tags to search for and
                         include in the results
    :param options: a dictionary of additional options (optional)
    :return: an iterator over ``(lineno, funcname, message, comments)``
             tuples
    :rtype: ``iterator``
    """
~~~
其次，你应该在setup.py中注册个方法作为`entry point`。如下：
~~~
def setup(...

    entry_points = """
    [babel.extractors]
    xxx = your.package:extract_xxx
    """,
~~~

### Translator Comments
First of all what are comments tags. Comments tags are excerpts of text to search for in comments, only comments, right before the python gettext calls, as shown on the following example:
~~~
# NOTE: This is a comment about `Foo Bar`
_('Foo Bar')
~~~
The comments tag for the above example would be NOTE:, and the translator comment for that tag would be This is a comment about `Foo Bar`.

The resulting output in the catalog template would be something like:
~~~
#. This is a comment about `Foo Bar`
#: main.py:2
msgid "Foo Bar"
msgstr ""
~~~
Now, you might ask, why would I need that?

Consider this simple case; you have a menu item called “manual”. You know what it means, but when the translator sees this they will wonder did you mean:

a document or help manual, or
a manual process?
This is the simplest case where a translation comment such as “The installation manual” helps to clarify the situation and makes a translator more productive.


## Command-Line Interface
Babel包含一个命令行接口和message catalogs一起工作，类似Linux/Unix系统下的gettext。
正确安装后，Babel提供一个叫做`pybabel`的脚本:
~~~
$ pybabel --help
usage: pybabel command [options] [args]

options:
  --version       查看版本信息
  -h, --help      查看帮助信息
  --list-locales  答应所有知道的区域信息
  -v, --verbose   [测试不可用]
  -q, --quiet     [测试不可用]

commands:
  compile  编译message catalogs成MO文件
  extract  从源文件提取message并生产POT文件
  init     从POT文件创建一个新的message catalogs
  update   更新POT文件的message
~~~
`pybabel`脚本提供了一些子命令来完成实际的工作。下面来描述一下这些子命令。

### compile
compile子命令用来把translation catalogs编译成二进制的MO文件:
~~~
$ pybabel compile --help
usage: pybabel compile [options]

compile message catalogs to MO files

options:
  -h, --help            show this help message and exit
  -D DOMAIN, --domain=DOMAIN
                        domain of MO and PO files (default 'messages')
  -d DIR, --directory=DIR
                        base directory of catalog files
  -l LOCALE, --locale=LOCALE
                        locale of the catalog
  -i FILE, --input-file=FILE
                        name of the input file
  -o FILE, --output-file=FILE
                        name of the output file (default
                        '<output_dir>/<locale>/LC_MESSAGES/<domain>.mo')
  -f, --use-fuzzy       also include fuzzy translations (default False)
  --statistics          print statistics about translations
~~~
如果directory被指定，但是output-file没有指定，则按如下方式输出:
~~~
<directory>/<locale>/LC_MESSAGES/<domain>.mo
~~~
如果input_file和local都没有指定，则命令会在根目录下寻找所有catalog文件并在相同目录下生成他们的MO文件。

### extract
这个子命令用来从源文件集合中提取可本地化messages
~~~
$ pybabel extract --help
usage: pybabel extract [options] dir1 <dir2> ...

extract messages from source files and generate a POT file

options:
  -h, --help            show this help message and exit
  --charset=CHARSET     charset to use in the output (default "utf-8")
  -k KEYWORDS, --keyword=KEYWORDS
                        keywords to look for in addition to the defaults. You
                        can specify multiple -k flags on the command line.
  --no-default-keywords
                        do not include the default keywords
  -F MAPPING_FILE, --mapping=MAPPING_FILE
                        path to the extraction mapping file
  --no-location         do not include location comments with filename and
                        line number
  --omit-header         do not include msgid "" entry in header
  -o OUTPUT, --output=OUTPUT
                        path to the output POT file
  -w WIDTH, --width=WIDTH
                        set output line width (default 76)
  --no-wrap             do not break long message lines, longer than the
                        output line width, into several lines
  --sort-output         generate sorted output (default False)
  --sort-by-file        sort output by file location (default False)
  --msgid-bugs-address=EMAIL@ADDRESS
                        set report address for msgid
  --copyright-holder=COPYRIGHT_HOLDER
                        set copyright holder in output
  -c TAG, --add-comments=TAG
                        place comment block with TAG (or those preceding
                        keyword lines) in output file. One TAG per argument
                        call
                        
~~~

### init
这个子命令基于POT文件创建一个新的translations catalog
~~~
$ pybabel init --help
usage: pybabel init [options]

create new message catalogs from a POT file

options:
  -h, --help            show this help message and exit
  -D DOMAIN, --domain=DOMAIN
                        domain of PO file (default 'messages')
  -i FILE, --input-file=FILE
                        name of the input file
  -d DIR, --output-dir=DIR
                        path to output directory
  -o FILE, --output-file=FILE
                        name of the output file (default
                        '<output_dir>/<locale>/LC_MESSAGES/<domain>.po')
  -l LOCALE, --locale=LOCALE
                        locale for the new localized catalog        
~~~

### update
更新POT文件
The update sub-command updates an existing new translations catalog based on a PO template file:
~~~
$ pybabel update --help
usage: pybabel update [options]

update existing message catalogs from a POT file

options:
  -h, --help            show this help message and exit
  -D DOMAIN, --domain=DOMAIN
                        domain of PO file (default 'messages')
  -i FILE, --input-file=FILE
                        name of the input file
  -d DIR, --output-dir=DIR
                        path to output directory
  -o FILE, --output-file=FILE
                        name of the output file (default
                        '<output_dir>/<locale>/LC_MESSAGES/<domain>.po')
  -l LOCALE, --locale=LOCALE
                        locale of the translations catalog
  --ignore-obsolete     do not include obsolete messages in the output
                        (default False)
  -N, --no-fuzzy-matching
                        do not use fuzzy matching (default False)
  --previous            keep previous msgids of translated messages (default
                        False)
~~~

## Distutils/Setuptools Integration
<http://babel.pocoo.org/en/latest/setup.html>