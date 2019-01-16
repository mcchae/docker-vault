# Vault

사용자가 암호 등을 안전하게 보관하기 위한 금고 (Safe 또는 Vault)를 만드는 개념은 쉬울 것 같으면 서도 쉽지 않습니다. 로컬에 어떤 식으로 저장을 하더라도 해당 소스가 공개되어 있고 저장하는 방식을 알게되면 역으로 해독이 가능하기 때문입니다.

그래서 시간을 들여 찾아 보니 원격으로 해당 기능을 제공하는 서비스가 여러 있었고 그 중에서도 가장 편하고 안전하게 저장하고 가져오는 등의 작업과 이미 dockerize 되어 있는 것을 찾았으니 바로, [HashiCorp](https://www.hashicorp.com) 사의 [Vault](https://www.vaultproject.io) 라는 것이었습니다.

[Securing secrets with python and vault](https://modularsystems.io/blog/securing-secrets-python-vault/) 블로그를 찾아서 확인하다가 [ryanhartje/containers의 consul-vault](https://github.com/ryanhartje/containers/tree/master/consul-vault)를 찾았고 해당 vault의 docker image 를 별도로 구성해 봅니다.

## Dockerfile
```sh
FROM debian:jessie

RUN apt-get update && \
    apt-get install ca-certificates curl unzip -y && \
    apt-get autoclean && \
    apt-get --purge -y autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV VAULT_VERSION 0.6.4

RUN bash -c 'mkdir -p /vault/conf.d'

RUN curl -Lo /vault/vault_${VAULT_VERSION}_linux_amd64.zip https://releases.hashicorp.com/vault/${VAULT_VERSION}/vault_${VAULT_VERSION}_linux_386.zip && \
    unzip /vault/vault_${VAULT_VERSION}_linux_amd64.zip -d /vault && \
    rm -rf /vault/vault_${VAULT_VERSION}_linux_amd64.zip && \
    chmod +x /vault/vault

EXPOSE 8200 

ENTRYPOINT ["/vault/vault"]

CMD []
```

## 의존성
[consul docker iamge](https://github.com/mcchae/docker-consul) 를 필요로 합니다.

## 참고
> * [Securing secrets with python and vault](https://modularsystems.io/blog/securing-secrets-python-vault/)
> * [What is HashiCorp Vault? How to Secure Secrets Inside Microservices](https://cloudacademy.com/blog/hashicorp-vault-how-to-secure-secrets-inside-microservices/)
> * [HashiCorp의 비밀정보 관리 도구 Vault의 구성](https://blog.outsider.ne.kr/1266)
> * [hvac python Vault module](https://hvac.readthedocs.io/en/stable/index.html)

