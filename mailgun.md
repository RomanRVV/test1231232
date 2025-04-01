## Как получить переменные окружения для Mailgun:

```shell
MAILGUN__API_KEY="ваш API ключ Mailgun"
MAILGUN__DOMAIN="ваш домен Mailgun"
MAILGUN__SIGNING_KEY="ваш ключ подписи Mailgun"
MAILGUN__BASE_URL="базовый url Mailgun"
MAILGUN__SENDER_EMAIL="ваш email-адрес отправителя"
MAILGUN__RETENTION_DAYS="срок хранения данных в бд"
```

Для получения переменных окружения mailgun нужно войти в аккаунт https://app.mailgun.com/

#### MAILGUN__API_KEY

В dashboard нажми API keys и добавь новый API ключ(увидеть старый ключ уже нельзя — можно только создать новый)
![3](https://github.com/user-attachments/assets/7c32eceb-1f07-4ab4-a3db-0c84878fb3e7)

#### MAILGUN__SIGNING_KEY

На той же странице скопируй SIGNING_KEY для вебхуков 
![4](https://github.com/user-attachments/assets/528741e3-fe12-4274-a518-e638917a692a)


#### MAILGUN__BASE_URL

Базовый URL для API Mailgun, по умолчанию используется https://api.mailgun.net

#### MAILGUN__DOMAIN и MAILGUN__SENDER_EMAIL

На главной странице слева раскрыть меню send и выбрать Domains, тут взять название домена для MAILGUN__DOMAIN. Для MAILGUN__SENDER_EMAIL можешь отправлять письма с любого email на домене, если домен подтвержден
![1](https://github.com/user-attachments/assets/3c5409a2-c8c7-4155-aaee-153223b0dd53)


#### MAILGUN__RETENTION_DAYS

Cрок хранения данных в днях, по истечении которого они могут быть удалены вручную через действие в админке. Значение по умолчанию — 90.


## Настройка вебхуков 

1 В меню Send выбери Webhooks, далее выбери свой домен и нажми Add webhook. 

![5](https://github.com/user-attachments/assets/60fa6d26-1db3-429b-822d-7038fde45327)

2 Введи URL(https://yourwebsite.com/mailgun-api/webhooks/), куда Mailgun будет отправлять данные

3 Для каждого события (например, delivered, failed, opened) укажи один и тот же URL


## Окружение

Создайте в кластере Secret mailgun с данными аккаунта `api_key` и `signing_key`.

Пример манифеста для секрета `mailgun`:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mailgun
  namespace: your-namespace
stringData:
  api_key: "your-api-key"
  signing_key: "your-signing-key"
```
Этот манифест должен быть создан и применён в кластере Kubernetes для корректной работы с Mailgun. Убедитесь, что данные аккаунта безопасно хранятся и не публикуются открыто.

Остальные настройки Mailgun хранятся в ConfigMap:
- `domain`
- `base_url`
- `sender_email`
- `retention_days`


