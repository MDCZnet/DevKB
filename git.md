# GIT

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
