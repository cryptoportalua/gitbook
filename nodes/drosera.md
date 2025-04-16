# Drosera

## Ручне встановлення

### Підготовка сервера

```bash
# Update server
sudo apt-get update && sudo apt-get upgrade -y && \
sudo apt install curl ufw iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

```bash
# Install Drosera cli
curl -L https://app.drosera.io/install | bash && source ~/.bashrc && droseraup
```

```bash
# Install Foundry
curl -L https://foundry.paradigm.xyz | bash && source ~/.bashrc && foundryup
```

### Деплой трапа

```bash
# Initialize Drosera project
mkdir my-drosera-trap
cd my-drosera-trap
```

Додаємо в перемінні свої дані Github для авторизації

```
GITHUB_EMAIL=
GITHUB_USERNAME=
```

```bash
git config --global user.email "$GITHUB_EMAIL"
git config --global user.name "$GITHUB_USERNAME"
```

```bash
forge init -t drosera-network/trap-foundry-template
```

```bash
# Install Bun
curl -fsSL https://bun.sh/install | bash && source ~/.bashrc && bun install && \
forge build
```

Зберігаємо в перемінну приватний ключ гаманця із тестовими токенами holesky

```bash
PRIV_KEY=
```

```bash
export DROSERA_PRIVATE_KEY="$PRIV_KEY"
```

```
drosera apply
```

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Вписуємо `ofc` та тиснемо `enter`

У разі успішного деплою ви побачите щось типу цього, транзакцію можна перевірити в експлоуері за посиланням з терміналу

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Переходимо за посиланням, підключаємо свій гаманець та тиснемо Traps Owned та свій Trap

{% embed url="https://app.drosera.io/" %}

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

Тиснемо Send Bloom Boost та відправляємо будь яку кількість ефіра для буста

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

Коли транзакція пройде успішно буст з'явиться у профілі

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

Повертаємось в термінал та запускаємо

```bash
drosera dryrun && cd
```

### Запуск ноди

```
```
