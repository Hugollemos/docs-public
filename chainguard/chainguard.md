📌 package:
Define metadados, como nome, versão, descrição, licença, e dependências de runtime.

yaml
Copiar
Editar
package:
  name: php
  version: 8.2.8
  epoch: 0
  description: the PHP programming language
  copyright:
    - license: PHP-3.01
  dependencies:
    runtime:
      - libxml2
🏗️ environment:
Lista as dependências de build, repositórios e chave pública.

yaml
Copiar
Editar
environment:
  contents:
    repositories:
      - https://packages.wolfi.dev/os
    keyring:
      - https://packages.wolfi.dev/os/wolfi-signing.rsa.pub
    packages:
      - build-base
      - libxml2-dev
      - curl-dev
      ...
🔄 pipeline:
Define etapas do build, como baixar o código, compilar e instalar.

yaml
Copiar
Editar
  - uses: fetch
    with:
      uri: https://example.com/lib-${{package.version}}.tar.gz
      expected-sha256: <checksum>
🧩 subpackages:
Cria subpacotes (ex: php-dev, php-curl) para manter a imagem principal mínima.

yaml
Copiar
Editar
  - name: php-dev
    description: PHP development headers
    pipeline:
      - uses: split/dev
🔁 Looping com range:
Permite gerar automaticamente subpacotes a partir de listas.

yaml
Copiar
Editar
data:
  - name: extensions
    items:
      curl: cURL
      gmp: GNU GMP
      ...

subpackages:
  - range: extensions
    name: "php-${{range.key}}"
    description: "The ${{range.value}} extension"
    pipeline:
      - runs: |
          ...
🔔 update:
Usado apenas se for enviar o pacote para o repositório Wolfi oficial (ajuda o CI a detectar novas versões).

yaml
Copiar
Editar
update:
  enabled: true
  github:
    identifier: php/php-src
