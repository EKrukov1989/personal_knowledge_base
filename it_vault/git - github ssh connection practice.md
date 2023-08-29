___
В этой статье описан метод соединения с github с помощью ssh на ubuntu 23.04.

___
### 1. Генерация ключей

Генерируем две пары ключей ([[ssh - ssh-keygen]]):
```bash
cd ~/.ssh
ssh-keygen -f ssh_github_auth_1_rsa_key
ssh-keygen -f ssh_github_sign_1_rsa_key
```

>[!tip]
>Каждый ключ снабжен номером, если ключи потеряются, то следующую пару нужно создать с другим номером.

Будут получены следующие файлы:
- `~/.ssh/ssh_github_auth_1_rsa_key`
- `~/.ssh/ssh_github_auth_1_rsa_key.pub`
- `~/.ssh/ssh_github_sign_1_rsa_key`
- `~/.ssh/ssh_github_sign_1_rsa_key.pub`
При генерации ключей они будут автоматически добавлены в ssh-agent.

>[!tip]
>Ключи и пароли к ним неплохо бы сохранить в безопасном месте.

Если ключи уже есть, то нужно добавить их в ssh-agent с помощью команды:
- `ssh add <keyfile>` ([[ssh - ssh-agent and ssh-add]])

___
### 2. Добавление ключей на github

Добавить ключи на github.com можно следующим образом:
- settings -> SSH and GPG keys -> SSH keys (конечно, только публичные!)

___
### 3. Known hosts

Информацию о публичных ключах github можно посмотреть здесь:
- [GitHub's SSH key fingerprints](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints)

Можно либо скопировать предлагаемые ключи в файл: `~/.ssh/known_hosts`,
либо при первом обращении к github проверить fingerprint хоста и тогда ключ также будет добавлен в known_hosts (хотя это зависит от настроек).

___
### 4. Клонирование репозитория

В целях проверки можно склонировать какой-нибудь репозиторий:
- `git clone git@github.com:EKrukov1989/my_red_black_tree.git`



