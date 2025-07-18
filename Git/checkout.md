 Используется для переключения между различными версиями целевого объекта. Команда `git checkout` работает с тремя различными объектами: файлами, коммитами и ветками.

Перемещение между коммитами

```bash
$ git сheckout <commit> | <tree-ish>
```

Состояние всего ==проекта== возвращается к указанному коммиту. При этом никакие коммиты, в отличие от команды `git reset`, не удалились.  Можно перейти обратно к актуальному коммиту, указав название текущей ветки, например `git checkout master`.

После перемещения на коммит, `HEAD` переходит в состояние `detached`, или в состояние открепленного указателя `HEAD`.  Это состояние предупреждает о том, что вся текущая работа «откреплена» от остальной части вашего проекта. Если вы начнете разрабатывать функцию, находясь в состоянии открепленного указателя `HEAD`, у вас не будет ветки, которая позволила бы вам вернуться к этой функции. Когда вы неизбежно переключитесь на другую ветку (например, чтобы слить код своей функции), вы уже никак не сможете сослаться на свою функцию. Любые изменения или коммиты сделанные в этом состоянии удаляются сборщиком мусора при переходе к другому коммиту.


Возврат файлов к состоянию, которое было на момент коммита. 

```bash
$ git сheckout <commit> -- <file1> <file2>
```

Без указания коммита возвращает все файлы `untracked` и `modified` к состоянию последнего коммита

```bash
$ git сheckout -- .
```

Название файла нужно указывать после `--`, чтобы одназначно указать, что после команды идет название файла, а не параметр.

```bash
$ git сheckout master
$ git сheckout -- master
```

В первом случае произойдет переключение на ветку `master`, во втором возвращение файла к состоянию последнего коммита.

Важно понимать, что `git checkout -- <file>` - опасная команда. Все локальные изменения в файле пропадут — Git просто заменит его версией из последнего коммита. Ни в коем случае не используйте эту команду, если вы не уверены, что изменения в файле вам не нужны.


### Убрать изменения из файлов в `staging area / index`

Нужно сначала перевести все файлы из состояния `staged` в состояние `modified`, а затем вернуть все файлы к состоянию последнего коммита

```bash
$ git reset
$ git сheckout -- .
```

Есть более современная команда для выполнения этих действий - `git restore`. Это альтернатива командам ранее выполненным командам.

```bash
$ git restore --staged .
$ git restore .
```