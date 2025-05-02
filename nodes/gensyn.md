# Gensyn

{% hint style="info" %}
Даний гайд протестовано на сервері без GPU (12 CPU / 64 RAM)
{% endhint %}

## Зміст

* [Скрипт](gensyn.md#skript)
* [Ручне встановлення](gensyn.md#ruchne-vstanovlennya)
  * [Підготовка сервера](gensyn.md#pidgotovka-servera)
  * [Встановлення ноди](gensyn.md#vstanovlennya-nodi)
  * [Підключення пошти](gensyn.md#pidklyuchennya-poshti)
* [Оновлення до v0.4.1](gensyn.md#onovlennya-do-v0.4.1)

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

### Підключення пошти

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

`Would you like to connect to the Testnet? [Y/n]` - Тиснемо Y

`Which swarm would you like to join (Math (A) or Math Hard (B))? [A/b]` - Для серверів CPU тиснемо A

`How many parameters (in billions)? [0.5, 1.5, 7, 32, 72]` - для слаших серверів чи vps обираєм `0,5`, для дедіків можна поставити `1,5`

Якщо ви бачите наступні логи, значить поки все вірно

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Тисните кнопку "Login" , після цього ви можете зареєструватись через google пошту, або вписати її вручну.

<figure><img src="../.gitbook/assets/Знімок екрана 2025-04-10 о 14.41.32.png" alt=""><figcaption></figcaption></figure>

Після того як залогінитесь у вас має з'явитись такий надпис.&#x20;

<figure><img src="../.gitbook/assets/Знімок екрана 2025-04-10 о 14.29.10.png" alt=""><figcaption></figcaption></figure>

> Would you like to push models you train in the RL swarm to the Hugging Face Hub?  y/ N \
> Тут натискаєте "n" і  Enter

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

&#x20;В логах з'явиться ваші три "name words" та ID вашої ноди. Можете їх виписати в таблицю для того що б подальшому можна було моніторити її в дашбоарді.

<figure><img src="../.gitbook/assets/Знімок екрана 2025-04-10 о 14.32.30.png" alt=""><figcaption></figcaption></figure>

Після того як все повністю прогрузиться, всі піри і з'єднання у вас мають з'явитись ось такі логи. По часу все залежить від сервера і напливу на мережу.&#x20;

<figure><img src="../.gitbook/assets/Знімок екрана 2025-04-10 о 17.36.23.png" alt=""><figcaption></figcaption></figure>

Гайд в подальшому буде оновлюватись, чекаємо апдейтів.

## Оновлення до v0.4.1

```bash
screen -x gensyn
```

```bash
cd ~/rl-swarm && source .venv/bin/activate
```

```bash
git reset --hard && \
git fetch --all && git checkout tags/v0.4.1
```

```bash
./run_rl_swarm.sh
```

Продовжуєм далі по [гайду](gensyn.md#pidklyuchennya-poshti)

{% embed url="https://t.me/cryptoportalua_chat" %}
