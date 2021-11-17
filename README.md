# devops-netology

Согласно файлу .gitignore, расположенному в поддиректории /terraform/, будут игнорироваться git-ом следующие файлы:

* все файлы в поддиректории /.terraform/
* файлы с расширением *.tfstate и с любым расширением, добавленным после этого, например файл 123.tfstate.bak
* файлы с именем crash.log, override.tf, override.tf.json
* файлы, имеющие в своём имени "_override.tf", "_override.tf.json"
* а также файлы с именами .terraformrc и terraform.rc


# Домашнее задание 2.4 "Инструменты GIT"

1.	Найдите полный хеш и комментарий коммита, хеш которого начинается на aefea.
	
 * хеш - aefead2207ef7e2aa5dc81a34aedf0cad4c32545
 * комментарий -  «Update CHANGELOG.md»

способ - git show aefea

     
2. Какому тегу соответствует коммит 85024d3?
	
 * v0.12.23

способ - git tag --points-at 85024d3

	
3. Сколько родителей у коммита b8d720? Напишите их хеши.
	
	Два родителя:
    * 56cd7859e05c36c06b56d013b55a252d0bb7e158
    * 9ea88f22fc6269854151c571162c5bcf958bee2b

Способ - git show b8d720 --pretty=%P


4. Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами v0.12.23 и v0.12.24.
	
	
* 33ff1c03b (tag: v0.12.24) v0.12.24
* b14b74c49 [Website] vmc provider links
* 3f235065b Update CHANGELOG.md
* 6ae64e247 registry: Fix panic when server is unreachable
* 5c619ca1b website: Remove links to the getting started guide's old location
* 06275647e Update CHANGELOG.md
* d5f9411f5 command: Fix bug when using terraform login on Windows
* 4b6d06cc5 Update CHANGELOG.md
* dd01a3507 Update CHANGELOG.md
* 225466bc3 Cleanup after v0.12.23 release

Способ - git log v0.12.23..v0.12.24 --oneline

5. Найдите коммит в котором была создана функция func providerSource, ее определение в коде выглядит так func providerSource(...) (вместо троеточего перечислены аргументы).
	
	
	8c928e83589d90a031f811fae52a81be7153e82f

Способ - git log -S "func providerSource("
	
6. Найдите все коммиты в которых была изменена функция globalPluginDirs.
	
	
	* 78b12205587fe839f10d946ea3fdc06719decb05
	* 52dbf94834cb970b510f2fba853a5b49ad9b1a46
	* 41ab0aef7a0fe030e84018973a64135b11abcd70
	* 66ebff90cdfaa6938f26f908c7ebad8d547fea17
	* 8364383c359a6b738a436d1b7745ccdce178df47

Способ - git grep "globalPluginDirs" 

dmitry@Dmitrijs-MacBook-Air terraform % git grep "globalPluginDirs"                    
commands.go:            GlobalPluginDirs: globalPluginDirs(),
commands.go:    helperPlugins := pluginDiscovery.FindPlugins("credentials", globalPluginDirs())
internal/command/cliconfig/config_unix.go:              // FIXME: homeDir gets called from globalPluginDirs during init, before
plugins.go:// globalPluginDirs returns directories that should be searched for
plugins.go:func globalPluginDirs() []string {

 Находим имя файла,  в котором была изменена функция, в данном случае это «plugins.go»
	
	
	git log -L '/globalPluginDirs/',/^}/:plugins.go

	

7. Кто автор функции synchronizedWriters?
	
      * commit 5ac311e2a
      * Author: Martin Atkins <mart@degeneration.co.uk>
      * Date:   Wed May 3 16:25:41 2017 -0700

Способ -  git log -S "synchronizedWriters" --oneline --pretty


# Домашнее задание по лекции "Работа в терминале (лекция 1)"
 
5. Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас Vagrant, какие аппаратные ресурсы ей выделены. Какие ресурсы выделены по-умолчанию?
      
По умолчанию выделено 1 ядро ЦП, 4 Мб Видеопамяти и 1024 Мб ОЗУ
  
6. Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: документация. Как добавить оперативной памяти или ресурсов процессора виртуальной машине?

Для того чтобы добавить ресурсов для виртуальной машины, необходимо использовать команду 

      config.vm.provider "virtualbox" do |v|

и указать необходимые значения 

               v.memory = 2048
               v.cpus = 2
            end

8.Ознакомиться с разделами man bash, почитать о настройках самого bash:

 * какой переменной можно задать длину журнала history, и на какой строчке manual это описывается?
 * что делает директива ignoreboth в bash?

Длина журнала задается через HISTSIZE, по умолчанию она имеет значение 500.  находится на 967 строке руководства 
    
HISTSIZE
              The number of commands to remember in the command history (see HIS-
              TORY below).  The default value is 500.

* ignorespace — не сохранять строки начинающиеся с символа <пробел>
* ignoredups — не сохранять строки, совпадающие с последней выполненной командой
* ignoreboth — использовать обе опции ‘ignorespace’ и ‘ignoredups’

9. В каких сценариях использования применимы скобки {} и на какой строчке man bash это описано?
  
 {} используются для выполнения одной команды несколько раз, 281 строка

10. С учётом ответа на предыдущий вопрос, как создать однократным вызовом touch 100000 файлов? Получится ли аналогичным образом создать 300000? Если нет, то почему?
 
         touch file{1..10000}
 
300000 создать не дает , argument is too long. 300000 - это максимальное количество файлов, которое может находиться в директории

11. В man bash поищите по /\[\[. Что делает конструкция [[ -d /tmp ]]

      -конструкция [[ -d /tmp ]] возвращает 0 или 1 в зависимости от выражения внутри ( В данном случае возвращает Истину, т. к. выражение внутри проверяет существует директория  /tmp )

12. Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе type -a bash в виртуальной машине наличия первым пунктом в списке:

             bash is /tmp/new_path_directory/bash
             bash is /usr/local/bin/bash
             bash is /bin/bash

(прочие строки могут отличаться содержимым и порядком) В качестве ответа приведите команды, которые позволили вам добиться указанного вывода или соответствующие скриншоты.


            vagrant@vagrant:/tmp$ mkdir new_path_directory
            vagrant@vagrant:/tmp/new_path_directory$ export PATH='/tmp/new_path_directory:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin'
            vagrant@vagrant:/tmp/new_path_directory$ cp /usr/bin/bash /tmp/new_path_directory/bash

         type -a bash
         bash is /tmp/new_path_directory/bash
         bash is /usr/bin/bash
         bash is /bin/bash

13. Чем отличается планирование команд с помощью batch и at?

  * at - используется для планирования выполнения задания на опредленное заданное время
  * batch  — для назначения одноразовых задач, которые должны выполняться, когда загрузка системы становится меньше 0,8.