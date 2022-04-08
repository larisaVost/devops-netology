1. Найдите полный хеш и комментарий коммита, хеш которого начинается на aefea
 git show --pretty=format:"%H %s" aefea  --no-patch
aefead2207ef7e2aa5dc81a34aedf0cad4c32545 Update CHANGELOG.md

2.Какому тегу соответствует коммит 85024d3?
теги:
 git tag -l --contains 85024d3
v0.12.23
v0.12.24
v0.12.25
v0.12.26
v0.12.27
v0.12.28
v0.12.29
v0.12.30
v0.12.31

ветки:
 git branch -a --contains 85024d3

  remotes/origin/cgriggs01-stable-pdp
  remotes/origin/july2020_pre0.13_stable-website_backup
  remotes/origin/v0.12

3.Сколько родителей у коммита b8d720? Напишите их хеши.
 git log -1 b8d720

2 родителя, это мерж коммит, хеши:  56cd7859e 9ea88f22f
commit b8d720f8340221f2146e4e4870bf2ee0bc48f2d5
Merge: 56cd7859e 9ea88f22f
Author: Chris Griggs <cgriggs@hashicorp.com>

или второй способ:
git show --pretty=raw b8d720

commit b8d720f8340221f2146e4e4870bf2ee0bc48f2d5
tree cec002aab630c8bc701cb85bc94e55e751cd2d8f
parent 56cd7859e05c36c06b56d013b55a252d0bb7e158
parent 9ea88f22fc6269854151c571162c5bcf958bee2b
author Chris Griggs <cgriggs@hashicorp.com> 1579657548 -0800
committer GitHub <noreply@github.com> 1579657548 -0800

4. Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами v0.12.23 и v0.12.24.
git log --pretty=format:"%H %s" ^v0.12.23 v0.12.24 
Раз в условии между - значит исключаю наличие коммитов в v0.12.23 
33ff1c03bb960b332be3af2e333462dde88b279e v0.12.24
b14b74c4939dcab573326f4e3ee2a62e23e12f89 [Website] vmc provider links
3f235065b9347a758efadc92295b540ee0a5e26e Update CHANGELOG.md
6ae64e247b332925b872447e9ce869657281c2bf registry: Fix panic when server is unreachable
5c619ca1baf2e21a155fcdb4c264cc9e24a2a353 website: Remove links to the getting started guide's old location
06275647e2b53d97d4f0a19a0fec11f6d69820b5 Update CHANGELOG.md
d5f9411f5108260320064349b757f55c09bc4b80 command: Fix bug when using terraform login on Windows
4b6d06cc5dcb78af637bbb19c198faff37a066ed Update CHANGELOG.md
dd01a35078f040ca984cdd349f18d0b67e486c35 Update CHANGELOG.md
225466bc3e5f35baa5d07197bbc079345b77525e Cleanup after v0.12.23 release

5. Найдите коммит в котором была создана функция func providerSource, ее определение в коде выглядит так func providerSource(...) (вместо троеточего перечислены аргументы).
git log -S "func providerSource(" --oneline

8c928e835 main: Consult local directories as potential mirrors of providers

6. Найдите все коммиты в которых была изменена функция globalPluginDirs.
К хешу коммита добавила автора и форматировала дату в привычный формат

git log -L :providerSource:provider_source.go --no-patch --pretty=format:"%h - %an, %ad" --date=format:"%d-%m-%Y %H:%M:%S"

5af1e6234 - Martin Atkins, 21-04-2020 16:28:59
92d6a30bb - Martin Atkins, 15-04-2020 11:48:24
8c928e835 - Martin Atkins, 02-04-2020 18:04:39

7. Кто автор функции synchronizedWriters ?
Сначала извлекаем коммиты, в которых эта функция добавлялась/удалялась
git log -S"synchronizedWriters" --oneline

bdfea50cc remove unused
fd4f7eb0b remove prefixed io
5ac311e2a main: synchronize writes to VT100-faker on Windows

По первому коммиту находим автора, который добавил функцию в synchronized_writers.go
git show --pretty=format:"%h - %an - %cn" 5ac311e2a --no-patch

5ac311e2a - Martin Atkins - Martin Atkins
Автор - Martin Atkins


