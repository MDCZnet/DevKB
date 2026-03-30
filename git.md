# GIT

## Nová verze do Worktree

Postup pro vytvoření nové verze (např. `v12`) jako kopie existující větve (např. `v11`):

```shell
# 1. Vytvořit větev v12 z v11 (spustit z hlavního worktree, např. da/)
git branch v12 v11

# 2. Přidat nový worktree pro v12
git worktree add ../da-v12 v12

# 3. Pushnout větev na GitHub a nastavit tracking
git -C ../da-v12 push -u origin v12

# Ověřit všechny worktrees
git worktree list
```

**Po vytvoření worktree pro Laravel projekt:**

```shell
cd ../da-v12
composer install
cp ../da-v11/.env .env          # zkopírovat a upravit .env
php artisan key:generate
npm install
npm run build                   # nutné! bez toho Vite manifest chyba
php artisan icons:cache         # nutné! bez toho výrazně pomalý (158KB cache SVG ikon)
php artisan event:cache
```

> Hlavní worktree (např. `da/`) je na větvi `main`. Nové verze jsou samostatné worktrees.

## Worktreee

```shell
# Nová worktree složka z aktualní branch
git worktree add ../da-v01 -b v01

    # Pro Laravel dále...
    composer install
    npm install

# Nová worktree složka prázdná
git worktree add --orphan ../da-v01 v01

# Chybějící worktree složka
# Ve složce vytovřit soubor `.git`
gitdir: C:/laragon/www/da/.git/worktrees/da-v11
```

## Výchozí

```shell
# Výchozí název větev pro nové repozitáře
git config --global init.defaultBranch main
```
