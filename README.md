# Monorepo de APIs - Exemplo Apigee X

Estrutura inicial simples para um repositório único (`apis`) contendo:

- Configurações compartilhadas de ambiente em `EdgeConfig/`
- Proxies do Apigee em `proxies/<nome-do-proxy>/apiproxy`
- Pipeline GitHub Actions único em `.github/workflows/apigee-ci-monorepo.yml`

Neste exemplo, só existe um proxy:

- `proxies/airports-cicd-v1/apiproxy`

Para adicionar novos proxies depois:

1. Copie a pasta `proxies/airports-cicd-v1` para um novo nome, por exemplo:
   `proxies/payments-v1`.
2. Ajuste o conteúdo do `pom.xml` dentro dessa nova pasta para refletir o nome do proxy,
   basepath, descrição etc.
3. No workflow `.github/workflows/apigee-ci-monorepo.yml`, adicione o novo nome na matriz:

```yaml
matrix:
  proxy:
    - airports-cicd-v1
    - payments-v1
```

A pipeline irá:

1. Rodar o job `apigee-config` uma vez, aplicando `EdgeConfig/edge.json` no ambiente.
2. Rodar o job `apigee-deploy` em matriz, uma vez por proxy listado em `matrix.proxy`,
   chamando Maven dentro de `proxies/<proxy>/`.
