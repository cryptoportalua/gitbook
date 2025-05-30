# Drosera

## Скрипт

```
source <(curl -s https://raw.githubusercontent.com/cryptoportalua/scripts/refs/heads/main/drosera)
```

## Ручне встановлення

Качаємо кран на сайті гугла `web3`

{% embed url="https://cloud.google.com/application/web3/faucet/ethereum/holesky" %}

<figure><img src="../.gitbook/assets/Знімок екрана 2025-04-17 о 09.57.40.png" alt=""><figcaption></figcaption></figure>

Вам має прийти 1 ETH в мережі Holesky, переважно більшість до 1 хв часу. І тепер готові для встановлення.

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

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

Вписуємо `ofc` та тиснемо `enter`

У разі успішного деплою ви побачите щось типу цього, транзакцію можна перевірити в експлоуері за посиланням з терміналу

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Переходимо за посиланням, підключаємо свій гаманець та тиснемо Traps Owned та свій Trap

{% embed url="https://app.drosera.io/" %}

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

Тиснемо **Send Bloom Boost** та відправляємо будь яку кількість ефіра для буста

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Чим більше ефіра тим більший буст, на що конкретно впливає буст поки не відомо, максимальна кількість більше 2 ETH

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

Вписуємо бажану кількість ефіра та тиснем підтвердити на сайті і в гаманці.

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

Коли транзакція пройде успішно буст з'явиться у профілі.

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

Повертаємось в термінал та запускаємо

```bash
drosera dryrun && cd
```

### Запуск ноди

```bash
# Edit drosera.toml
cd ~/my-drosera-trap && source ~/.bashrc
sed -i '/^private_trap/d' ~/my-drosera-trap/drosera.toml
sed -i '/^whitelist/d' ~/my-drosera-trap/drosera.toml
```

Записуємо адресу свого гаманця та його приватний ключ

```bash
ADDRESS=
PRIV_KEY=
```

```bash
echo "private_trap = true" >> ~/my-drosera-trap/drosera.toml
echo "whitelist = [\"$ADDRESS\"]" >> ~/my-drosera-trap/drosera.toml
export DROSERA_PRIVATE_KEY="$PRIV_KEY"
```

{% hint style="info" %}
Треба почекати 10 хв після цих команд і можете робити наступну.
{% endhint %}

```
drosera apply
```

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

Вписуємо `ofc` та тиснемо `enter`

Перевіряємо наш дажборд, trap має стати `Private`

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

```bash
cd ~/my-drosera-trap && curl -LO https://github.com/drosera-network/releases/releases/download/v1.16.2/drosera-operator-v1.16.2-x86_64-unknown-linux-gnu.tar.gz && \
tar -xvf drosera-operator-v1.16.2-x86_64-unknown-linux-gnu.tar.gz && \
cp drosera-operator /usr/bin
```

Для коректної роботи ноди рекомендуємо використовувати приватні RPC Holesky, їх можна отримати на наступних сервісах:

1. [https://blastapi.io/](https://blastapi.io/)
2. [https://www.alchemy.com/](https://www.alchemy.com/)
3. [https://www.infura.io/](https://www.infura.io/)
4. [https://www.quicknode.com/](https://www.quicknode.com/)

Зберігаємо 2 RPC в перемінні, один буде основним, інший резервним. Ми використовуємо 2 акаунти blastapi

```bash
HOLESKY_RPC="ваш рпс 1"
HOLESKY_RPC1="ваш рпс 2"
```

```bash
drosera-operator register --eth-rpc-url $HOLESKY_RPC --eth-private-key $DROSERA_PRIVATE_KEY
```

```bash
ufw allow 31313
ufw allow 31314
```

```bash
sudo tee /etc/systemd/system/drosera.service > /dev/null <<EOF
[Unit]
Description=Drosera Node Service
After=network-online.target

[Service]
User=$USER
Restart=always
RestartSec=15
LimitNOFILE=65535
ExecStart=$(which drosera-operator) node --db-file-path /root/.drosera.db --network-p2p-port 31313 --server-port 31314 \\
    --eth-rpc-url $HOLESKY_RPC \\
    --eth-backup-rpc-url $HOLESKY_RPC1 \\
    --drosera-address 0xea08f7d533C2b9A62F40D5326214f39a8E3A32F8 \\
    --eth-private-key $DROSERA_PRIVATE_KEY \\
    --listen-address 0.0.0.0 \\
    --network-external-p2p-address $(curl -s https://api.ipify.org) \\
    --disable-dnr-confirmation true
EOF
```

```bash
sudo systemctl daemon-reload
sudo systemctl enable drosera
sudo systemctl restart drosera
```

```bash
journalctl -u drosera -f -o cat
```

Ідем на сайт і копіюємо адресу трапа, якщо ви хочете зробити його через термінал. Абож ви можете зробити це на сайті  настинувши клавішу `Opt In+`

<figure><img src="../.gitbook/assets/Знімок екрана 2025-04-17 о 11.58.18.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Знімок екрана 2025-04-17 о 11.58.50.png" alt=""><figcaption></figcaption></figure>

І після того робимо Opt In  якщо ви робете через термінал.

```bash
drosera-operator optin \
--eth-rpc-url https://ethereum-holesky-rpc.publicnode.com \
--eth-private-key $DROSERA_PRIVATE_KEY \
--trap-config-address "ввести адресу трапа"
```

Після того у вас дашбоарді має зявитись ось такі зелені блоки, перше підуть червоні просто трохи почекайте.

<figure><img src="../.gitbook/assets/Знімок екрана 2025-04-17 о 12.03.49.png" alt=""><figcaption></figcaption></figure>

## Update

Перше треба оновити CLI Drosera

```
curl -L https://app.drosera.io/install | bash
```

```
source /root/.bashrc
```

```
droseraup
```

У вас має з'явитись логи з версією v.1.17.1 Drosera&#x20;

<figure><img src="../.gitbook/assets/Знімок екрана 2025-05-08 о 21.40.25.png" alt=""><figcaption></figcaption></figure>

Тобі потрібно замінити попередній Seed Node RPC, вказаний у полі `drosera_rpc` у файлі `drosera.toml`, на новий Seed Node.

```
cd my-drosera-trap
```

```
nano drosera.toml
```

<figure><img src="../.gitbook/assets/Знімок екрана 2025-05-08 о 21.44.02.png" alt=""><figcaption></figcaption></figure>

Тобі потрібно замінити адресу `https://seed-node.testnet.drosera.io` на нову Seed Node адресу: `https://relay.testnet.drosera.io` у полі `drosera_rpc` файлу `drosera.toml`.

### Зробит веріф  трап адреси

У файлі `drosera.toml` переконайся, що є рядок, у якому вказана адреса твоєї пастки (trap), у такому форматі 0x341......

<figure><img src="../.gitbook/assets/Знімок екрана 2025-05-08 о 21.48.12.png" alt=""><figcaption></figcaption></figure>

У нас має бути вже адреса у файлі, якщо там немає адреси то ви можете скопіювати його на сайті  і вставити у файл як зазначено вище.

Далі треба зробити Re-Apply Drosera config

```
DROSERA_PRIVATE_KEY=xxxxxxxx drosera apply
```

```
xxxxxxxx- це ваш пріват кей
після цього маєте вести у консоль "ofc"
```

<figure><img src="../.gitbook/assets/Знімок екрана 2025-05-08 о 21.53.35.png" alt=""><figcaption></figcaption></figure>

Після цього ви можете зайти в дашбоард на сайті. І ви маєто побачити зелені блоки.

<figure><img src="../.gitbook/assets/Знімок екрана 2025-05-08 о 22.05.39.png" alt=""><figcaption></figcaption></figure>

Далі очікуємо апдейтів подальших можете слідкувати у нашому телеграм каналі.

{% embed url="https://t.me/cryptoportalua_chat" %}
