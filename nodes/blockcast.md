# Blockcast

**Що таке Blockcast?**\
Blockcast — це децентралізоване рішення для доставки контенту, що використовує технологію DePIN (децентралізовані мережі фізичної інфраструктури), аби змінити надто централізовану індустрію CDN (мереж доставки контенту). Хоча CDN з технічної точки зору є розподіленими, бізнес-модель цих мереж контролюється кількома великими гравцями. Blockcast прагне повернути контроль спільноті, зробивши інфраструктуру інтернету по-справжньому децентралізованою та власністю громади.

**Фінансування:** $2,85 мільйона

{% hint style="info" %}
Даний гайд протестовано на сервері без GPU (12 CPU / 64 RAM)
{% endhint %}

## Зміст

* [Скрипт](blockcast.md#skript)
* [Ручне встановлення](blockcast.md#ruchne-vstanovlennya)
  * [Підготовка сервера](blockcast.md#pidgotovka-servera)
  * [Встановлення ноди](blockcast.md#vstanovlennya-nodi)
  * [Реєстрація ноди](blockcast.md#teper-reyestruyemo-vashu-nodu-na-saiti)

## Скрипт

```bash
source <(curl -s https://raw.githubusercontent.com/cryptoportalua/scripts/refs/heads/main/blockcast)
```

## Ручне встановлення

Для початку треба зареєструвати аккаунт в дашбоарді на їхньому сайті.&#x20;

{% embed url="https://app.blockcast.network/" %}

<figure><img src="../.gitbook/assets/Знімок екрана 2025-05-28 о 00.16.27.png" alt=""><figcaption></figcaption></figure>

Також на сайті треба обов'язково залінкувати гаманець Phantom. Його можна залінкувати в секції профіля.

<figure><img src="../.gitbook/assets/Знімок екрана 2025-05-28 о 00.18.06.png" alt=""><figcaption></figcaption></figure>

### Підготовка сервера

```bash
# Update server
sudo apt-get update && sudo apt-get upgrade -y
sudo apt install iptables-persistent -y
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip -y
```

```bash
# Install docker
source <(curl -s https://raw.githubusercontent.com/cryptoportalua/scripts/refs/heads/main/utils/docker)
```

### Встановлення ноди

```bash
git clone https://github.com/Blockcast/beacon-docker-compose.git
```

```bash
cd beacon-docker-compose
```

```bash
docker compose up -d
```

Також перевірити логи  ноди. Вони мають бути ось такі.

<figure><img src="../.gitbook/assets/Знімок екрана 2025-05-28 о 00.25.07.png" alt=""><figcaption></figcaption></figure>

```
docker compose logs -fn 1000
```

### Тепер давайте згенеруємо hardware та challenge key:

```
docker compose exec blockcastd blockcastd init
```

Якщо все правильно зробили то у вас в терміналі має згенеруватись ці ключі.

<figure><img src="../.gitbook/assets/Знімок екрана 2025-05-28 о 00.26.13.png" alt=""><figcaption></figcaption></figure>

**Hardware ID** — це унікальний публічний ідентифікатор вашого пристрою.\
**Challenge Key** — це публічний ключ у форматі Solana, який є унікальним для вашого пристрою.\
Ці два ключі збережіть собі в таблицю для подальшого підтвердження власності вашої ноди.

{% hint style="warning" %}
Зробіть резервну копію вашого приватного ключа (розташований у `~/.blockcast/certs/gw_challenge.key`) і зберігайте його разом із Hardware ID у безпечному місці, інакше ви втратите можливість підтвердити право власності на цей пристрій.
{% endhint %}

Також для реєстрації вашої ноди знадобиться локація вашого сервера. Цією командою ви можете визначити де саме знаходиться ваш сервер.

```
curl -s https://ipinfo.io | jq '.city, .region, .country, .loc'
```

### Тепер реєструємо вашу ноду на сайті. &#x20;

Разом з ключами  в терміналі у вас згенерувалось посилання для реєстрації. Це **`Registration URL`**

<figure><img src="../.gitbook/assets/Знімок екрана 2025-05-28 о 00.26.13.png" alt=""><figcaption></figcaption></figure>

Копіюйте це посилання і вставте його в браузері. У вас одразу автоматично підтягнуться ці два ключі і тепер треба ввести `Node Name` та `Location`. Назву ви можете вибрати собі самостійно, а локацію ви можете вказати з пункту визначення адреси в терміналі.&#x20;

<figure><img src="../.gitbook/assets/Знімок екрана 2025-05-28 о 00.31.33.png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
Після того як пройшла реєстрація, треба почекати від 2 до 5 хв і ваша нода стане онлайн.
{% endhint %}

<figure><img src="../.gitbook/assets/Знімок екрана 2025-05-28 о 00.32.48.png" alt=""><figcaption></figcaption></figure>

### В подальшому гайду буде оновлюватись по мірі поступанні нових версій нод. За всіма новинами ви можете слідкувати в нашому телеграмі.

{% embed url="https://t.me/cryptoportalua_chat" %}
