### Installing TypeScript

```bash
### globally ### 
npm install -g typescript

### in project ### 
npm i -D typescript
```

### Create tsconfig.json

```bash
tsc --init
```

Команда `tsc` без аргументов использует файл `tsconfig.json` в текущем каталоге для определения, какие файлы TypeScript компилировать и какие настройки использовать при компиляции.

Однако, когда вы указываете конкретный файл TypeScript в качестве аргумента для `tsc` (например, `tsc index.ts`), `tsc` компилирует только этот файл и игнорирует файл `tsconfig.json`. Это происходит потому, что `tsc` предполагает, что вы хотите компилировать только этот файл, а не все файлы, указанные в `tsconfig.json`.

Если вы хотите, чтобы `tsc` использовал настройки из `tsconfig.json` при компиляции конкретного файла, вы можете использовать опцию `--project` или `-p`, чтобы указать каталог, содержащий `tsconfig.json`. Например:

```bash
tsc --project . index.ts
```

В этом примере `.` означает текущий каталог, поэтому `tsc` будет использовать `tsconfig.json` из текущего каталога при компиляции `index.ts`.

### Watch input files

```bash
tsc --watch
```