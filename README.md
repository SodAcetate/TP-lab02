## Laboratory work II

Данная лабораторная работа посвещена изучению систем контроля версий на примере **Git**.

```bash
$ open https://git-scm.com
```

## Tasks

- [ ] 1. Создать публичный репозиторий с названием **lab02** и с лиценцией **MIT**
- [ ] 2. Сгенирировать токен для доступа к сервису **GitHub** с правами **repo**
- [ ] 3. Ознакомиться со ссылками учебного материала
- [ ] 4. Выполнить инструкцию учебного материала
- [ ] 5. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```sh
$ export GITHUB_USERNAME=<имя_пользователя>
$ export GITHUB_EMAIL=<адрес_почтового_ящика>
$ export GITHUB_TOKEN=<сгенирированный_токен>
$ alias edit=<nano|vi|vim|subl>
```

```sh
$ cd ${GITHUB_USERNAME}/workspace
$ source scripts/activate
```

```sh
$ mkdir ~/.config
$ cat > ~/.config/hub <<EOF
github.com:
- user: ${GITHUB_USERNAME}
  oauth_token: ${GITHUB_TOKEN}
  protocol: https
EOF
$ git config --global hub.protocol https
```

```sh
$ mkdir projects/lab02 && cd projects/lab02
$ git init
$ git config --global user.name ${GITHUB_USERNAME}
$ git config --global user.email ${GITHUB_EMAIL}
# check your git global settings
$ git config -e --global
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab02.git
$ git pull origin master
$ touch README.md
$ git status
$ git add README.md
$ git commit -m"added README.md"
$ git push origin master
```

Добавить на сервисе **GitHub** в репозитории **lab02** файл **.gitignore**
со следующем содержимом:

```sh
*build*/
*install*/
*.swp
.idea/
```

```sh
$ git pull origin master
$ git log
```

```sh
$ mkdir sources
$ mkdir include
$ mkdir examples
$ cat > sources/print.cpp <<EOF
#include <print.hpp>

void print(const std::string& text, std::ostream& out)
{
  out << text;
}

void print(const std::string& text, std::ofstream& out)
{
  out << text;
}
EOF
```

```sh
$ cat > include/print.hpp <<EOF
#include <fstream>
#include <iostream>
#include <string>

void print(const std::string& text, std::ofstream& out);
void print(const std::string& text, std::ostream& out = std::cout);
EOF
```

```sh
$ cat > examples/example1.cpp <<EOF
#include <print.hpp>

int main(int argc, char** argv)
{
  print("hello");
}
EOF
```

```sh
$ cat > examples/example2.cpp <<EOF
#include <print.hpp>

#include <fstream>

int main(int argc, char** argv)
{
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF
```

```sh
$ edit README.md
```

```sh
$ git status
$ git add .
$ git commit -m"added sources"
$ git push origin master
```

## Report

```sh
$ cd ~/workspace/
$ export LAB_NUMBER=02
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER}.git tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```

## Homework

> Здесь я привёл пример выполнения лабы. Для приличия пишите код в **hello_world.cpp** сами!

### Part I

1. Создайте пустой репозиторий на сервисе github.com (или gitlab.com, или bitbucket.com).
2. Выполните инструкцию по созданию первого коммита на странице репозитория, созданного на предыдещем шаге.

> Создаём директорию для лабы и открываем терминал в ней.
> 
> ```
$ echo "# TP-lab02" >> README.md
$ git init
$ git add README.md
$ git commit -m "first commit"
$ git branch -M main
$ git remote add origin https://github.com/<user>/TP-lab02.git
$ git push -u origin main
> ```

3. Создайте файл `hello_world.cpp` в локальной копии репозитория (который должен был появиться на шаге 2). Реализуйте программу **Hello world** на языке C++ используя плохой стиль кода. Например, после заголовочных файлов вставьте строку `using namespace std;`.

> Создаём файл hello_world.cpp. В него пишем:
> ```cpp
> include <iostream>
> using namespace std;
> int main() {
> cout << "Hello world!";
> }
> ```

4. Добавьте этот файл в локальную копию репозитория.

> ```sh
> $ git add hello_world.cpp
> ```

5. Закоммитьте изменения с *осмысленным* сообщением.

> ```sh
> $ git commit -m "added hello_world.cpp"
> ```

6. Изменитьте исходный код так, чтобы программа через стандартный поток ввода запрашивалось имя пользователя. А в стандартный поток вывода печаталось сообщение `Hello world from @name`, где `@name` имя пользователя.

> В файле hello_world.cpp:
> ```cpp
> include <iostream>
> include <string>
> using namespace std;
> int main() {
> cout << "Hello world!";
> string name;
> cout << "Please enter name";
> cin >> name;
> cout << "Hello world from " << name;
> }
> ```

7. Закоммитьте новую версию программы. Почему не надо добавлять файл повторно `git add`?

> Вообще-то надо)
> ```sh
> $ git add hello_world.cpp
> $ git commit -m "updated hello_world.cpp to ask for name"
> ```

8. Запуште изменения в удалёный репозиторий.

> ```sh
> $ git push origin main
> ```

9. Проверьте, что история коммитов доступна в удалёный репозитории.

> Просто заходим на гитхаб и смотрим что появилось два коммита.

### Part II

**Note:** *Работать продолжайте с теми же репоззиториями, что и в первой части задания.*
1. В локальной копии репозитория создайте локальную ветку `patch1`.

> ```sh
> $ git branch patch1
> $ git checkout patch1
> ```

2. Внесите изменения в ветке `patch1` по исправлению кода и избавления от `using namespace std;`.

> В файле hello_world.cpp:
> ```cpp
> include <iostream>
> include <string>
> int main() {
> std::string name;
> std::cout << "Please enter name";
> std::cin >> name;
> std::cout << "Hello world from " << name;
> }
> ```
>

3. **commit**, **push** локальную ветку в удалённый репозиторий.

> ```sh
> $ git add hello_world.cpp
> $ git commit -m "patched hello_world.cpp"
> $ git push origin patch1
> ```

4. Проверьте, что ветка `patch1` доступна в удалёный репозитории.

> Заходим на гитхаб, переключаемся на ветку patch1 и смотрим глазками

5. Создайте pull-request `patch1 -> master`.

> Жмём **Compare & pull request** -> **Create pull request**

6. В локальной копии в ветке `patch1` добавьте в исходный код комментарии.

> В файле hello_world.cpp:
> ```cpp
> include <iostream>
> include <string>
> int main() {
> std::string name;
> // Бабушка технарь, дед гуманитарий
> std::cout << "Please enter name";
> std::cin >> name;
> std::cout << "Hello world from " << name;
> }
> ```

7. **commit**, **push**.

> ```sh
> $ git add hello_world.cpp
> $ git commit -m "added comment"
> $ git push origin patch1
> ```

8. Проверьте, что новые изменения есть в созданном на **шаге 5** pull-request

> Проверяем в браузере

9. В удалённый репозитории выполните  слияние PR `patch1 -> master` и удалите ветку `patch1` в удаленном репозитории.

> На страничке реквеста жмём **Merge pull request**
> 
> На главной страничке репозитория жмём **2 branches**
> 
> Удаляем ветку **patch1**

10. Локально выполните **pull**.

> ```sh
> $ git checkout main
> $ git pull origin main
> ```

11. С помощью команды **git log** просмотрите историю в локальной версии ветки `master`.

> ```sh
> $ git log
> ```

12. Удалите локальную ветку `patch1`.

> ```sh
> $ git branch -d patch1
> ```

### Part III

**Note:** *Работать продолжайте с теми же репоззиториями, что и в первой части задания.*
1. Создайте новую локальную ветку `patch2`.

> ```sh
> $ git branch patch2
> $ git checkout branch2
> ```

2. Измените *code style* с помощью утилиты [**clang-format**](http://clang.llvm.org/docs/ClangFormat.html). Например, используя опцию `-style=Mozilla`.

> ```sh
> $ sudo apt install clang-format
> $ clang-format hello_world.cpp -style=Mozilla -i hello_world.cpp
> ```

3. **commit**, **push**, создайте pull-request `patch2 -> master`.

> ```sh
> $ git add hello_world.cpp
> $ git commit -m "changed codestyle"
> $ git push origin patch2
> ```
>
> Затем заходим на гитхаб и жмём **Compare & pull request** -> **Create pull request**

4. В ветке **master** в удаленном репозитории измените комментарии, например, расставьте знаки препинания, переведите комментарии на другой язык.

> В гитхабе открываем hello_world.cpp, редактируем и сохраняем изменения, нажав **Commit changes**. 

5. Убедитесь, что в pull-request появились *конфликтны*.

> **Pull requests** -> наш пулл-реквест
>
> Должно появиться предупреждение "This branch has conflicts that must be resolved"
>
> Если этого не произошло, предыдущие шаги были выполнены неправильно


6. Для этого локально выполните **pull** + **rebase** (точную последовательность команд, следует узнать самостоятельно). **Исправьте конфликты**.

> ```sh
> $ git pull origin main --rebase
> ```
> Процесс ещё не завершён. Откройте файл hello_world, он будет выглядеть примерно так:
> ```cpp
> <<<<<<< HEAD
> include <iostream>
> include <string>
> int main() {
> std::string name;
> // Я изобрёл универсальный комментарий
> std::cout << "Please enter name";
> std::cin >> name;
> std::cout << "Hello world from " << name;
> =======
> include<iostream> include<string>
> int
> main()
> {
>   std::string name;
>   // Бабушка технарь, дед гуманитарий
>   std::cout << "Please enter name";
>   std::cin >> name;
>   std::cout << "Hello world from " << name;
> >>>>>>> changed codestyle
> }
> ```
> Здесь надо глазками посмотреть, чем файлы отличаются и ручками объединить их.
> Для этого в одной из копий кода напишите изменения из второй копии кода, затем удалите вторую копию кода и весь мусор.
>
> Должно получиться что-то вроде этого:
>
> ```cpp
> include<iostream> include<string>
> int
> main()
> {
>   std::string name;
>   // Я изобрёл универсальный комментарий
>   std::cout << "Please enter name";
>   std::cin >> name;
>   std::cout << "Hello world from " << name;
> }
> ```
> 
> ```sh
> $ git rebase --continue
> ```

7. Сделайте *force push* в ветку `patch2`

> **force-push** полностью перезаписывает историю ветки
>
> ```sh
> $ git push -f origin patch2
> ```

8. Убедитель, что в pull-request пропали конфликтны. 
9. Вмержите pull-request `patch2 -> master`.

> **Merge pull request**

## Links

- [hub](https://hub.github.com/)
- [GitHub](https://github.com)
- [Bitbucket](https://bitbucket.org)
- [Gitlab](https://about.gitlab.com)
- [LearnGitBranching](http://learngitbranching.js.org/)

```
Copyright (c) 2015-2021 The ISC Authors
```
