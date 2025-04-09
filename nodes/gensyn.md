# Gensyn

{% hint style="info" %}
Даний гайд протестовано на сервері без GPU (12 CPU / 64 RAM)
{% endhint %}

## Скрипт

```bash
screen -S gensyn
```

```bash
source <(curl -s https://raw.githubusercontent.com/cryptoportalua/scripts/refs/heads/main/gensyn)
```

`Would you like to connect to the Testnet? [Y/n]` -  вводимо "Y" та Enter

Переходимо до розділу ["Підключення пошти"](gensyn.md#pidklyuchennya-poshti)

## Ручне встановлення

### Підготовка сервера

```bash
# Update server
sudo apt-get update && \
sudo apt install curl wget git build-essential python3 python3-pip python3-venv screen -y
```

```bash
# Install docker
source <(curl -s https://raw.githubusercontent.com/cryptoportalua/scripts/refs/heads/main/utils/docker)
```

```bash
# Install Node.js & yarn
source <(curl -s https://raw.githubusercontent.com/cryptoportalua/scripts/refs/heads/main/utils/node_js)
```

```bash
# Open port
ufw allow 3000
```

### Встановлення ноди

```bash
screen -S gensyn
```

```bash
# Install gensyn
git clone https://github.com/gensyn-ai/rl-swarm/ && \
cd /root/rl-swarm && python3 -m venv .venv && source .venv/bin/activate
```

```bash
cd modal-login && yarn install && \
yarn upgrade && yarn add next@latest && yarn add viem@latest && \
cd .. && ./run_rl_swarm.sh
```

`Would you like to connect to the Testnet? [Y/n]` -  вводимо "Y" та Enter

### Підключення пошти

Якщо ви бачите наступні логи, значить поки все вірно

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Налаштовуємо тунель між сервером та своїм компом щоб підключити пошту.

Відкриваємо звичайний термінал на своєму комп'ютері та вводимо команду

> ```bash
> # Замініть значення на свої
> # Server_IP - IP сервера із нодою
> # key_directory - шлях до ssh ключа на вашому комп'ютері
> ```

```bash
# Якщо вхід на сервер по лоніну
ssh -L 3000:localhost:3000 root@Server_IP
```

```bash
# Якщо використовується ssh ключ
ssh -L 3000:localhost:3000 -i key_directory root@Server_IP
```

<details>

<summary>Редагування доступу для ключа ssh (для windows)</summary>

* Відкриваємо властивості файлу ключа
* Переходимо на вкладку "Безпека"
* Тиснемо "Змінити" й додаємо свого користувача, ставимо йому галочку "Повний доступ"
* Переходимо у "Додатково" та вимикаємо успадкування

</details>

Після створення тунелю можна переходити у браузер та відкриваємо сторінку із `localhost:3000`

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

