---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-24"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# CLI do {{site.data.keyword.registrylong_notm}}
{: #containerregcli}

É possível usar a CLI do {{site.data.keyword.registrylong}}, que é fornecida no plug-in da CLI `container-registry`, para gerenciar seu registro e seus recursos da sua conta do {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

**Pré-requisitos**

* Instale o [{{site.data.keyword.Bluemix_notm}} CLI ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli/index.html#overview). O prefixo para executar comandos usando a CLI do {{site.data.keyword.Bluemix_notm}} é `ibmcloud`.

* Antes de executar os comandos de registro, efetue login no {{site.data.keyword.Bluemix_notm}}
 com o comando `ibmcloud login` para gerar um token de acesso e autenticar sua sessão.

Na linha de comandos, você é notificado quando as atualizações para os plug-ins da CLI `ibmcloud` e da CLI `container-registry` estão disponíveis. Assegure-se de manter sua CLI atualizada para que seja possível usar todos os comandos e sinalizadores disponíveis.

Se você desejar visualizar a versão atual do plug-in da CLI `container-registry`, execute `ibmcloud plugin list`.

Para descobrir sobre como usar a CLI do {{site.data.keyword.registrylong_notm}}, veja [Introdução ao {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/index.html#index).

Para obter mais informações sobre a plataforma IAM e as funções de acesso de serviço que são necessárias para alguns comandos, consulte [Gerenciando acesso de usuário com o Identity and Access Management](/docs/services/Registry/iam.html#iam).

Não coloque informações pessoais em imagens de contêiner, nomes de namespace, campos de descrição (por exemplo, em tokens de registro) ou em qualquer dado de configuração de imagem (por
exemplo, nomes ou rótulos de imagem).
{:tip}

## ` ibmcloud cr api `
{: #bx_cr_api}

Retorna os detalhes sobre o terminal de API de registro com relação ao qual os comandos são executados.

```
ibmcloud cr api
```
{: codeblock}

**Pré-requisitos**

Nenhum

## ` ibmcloud cr build `
{: #bx_cr_build}

Constrói uma imagem do Docker no {{site.data.keyword.registrylong_notm}}.

```
ibmcloud cr build [--no-cache] [--pull] [--quiet | -q] [--build-arg KEY=VALUE ...] [--file FILE | -f FILE] --tag TAG DIRECTORY
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de acesso para usar o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using).

**Opções de comandos**
<dl>
<dt>`DIRECTORY`</dt>
<dd>O local de seu contexto de compilação, que contém o Dockerfile e os arquivos de pré-requisito. Se você executar o comando quando seu diretório ativo estiver configurado para onde seu contexto de compilação estiver armazenado, será possível substituir `DIRECTORY` por um ponto (.).</dd>
<dt>`-- no-cache`</dt>
<dd>(Opcional) Se especificado, camadas de imagem em cache de construções anteriores não serão usadas nesta compilação.</dd>
<dt>`-- pull`</dt>
<dd>(Opcional) Se especificado, as imagens base são puxadas mesmo se uma imagem com uma tag de correspondência já existe no host de construção.</dd>
<dt>`--quiet`, `-q`</dt>
<dd>(Opcional) Se especificado, a saída de construção é suprimida, a menos que ocorra um erro.</dd>
<dt>`-- build-arg KEY=VALUE`</dt>
<dd>(Opcional) Especifique um argumento de construção adicional no formato `'KEY=VALUE'`. Múltiplos argumentos de compilação podem ser especificados, incluindo este parâmetro, múltiplas vezes. O valor de cada argumento de construção está disponível como uma variável de ambiente quando você especifica uma linha ARG que corresponde à chave em seu Dockerfile.</dd>
<dt>`--file FILE`, `-f FILE`</dt>
<dd>(Opcional) Se você usar os mesmos arquivos para múltiplas construções, será possível escolher um caminho para um Dockerfile diferente. Especifique o local do Dockerfile relativo ao contexto de compilação. Se não especificado, o padrão será `PATH/Dockerfile`, em que PATH é a raiz do contexto de compilação.</dd>
<dt>`--tag TAG`, `-t TAG`</dt>
<dd>O nome completo da imagem que você deseja construir, que inclui a URL do registro e o namespace.</dd>
</dl>

**Exemplo**

Construa uma imagem do Docker que não use um cache de construção de construções anteriores, em que a saída de construção seja suprimida, a tag seja *`registry.ng.bluemix.net/birds/bluebird:1`* e o diretório seja seu diretório ativo.

```
ibmcloud cr build --no-cache --quiet --tag registry.ng.bluemix.net/birds/bluebird:1 .
```
{: pre}

## ` ibmcloud cr UNK-add `
{: #bx_cr_exemption_add}

Crie uma isenção para um problema de segurança. É possível criar uma isenção para um problema de segurança que se aplica a diferentes escopos. O escopo pode ser a conta, o namespace, o repositório ou a tag.

```
ibmcloud cr exemption-add --scope SCOPE --issue-type ISSUE_TYPE --issue-id ISSUE_ID
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de acesso para configurar o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_configure).

**Opções de comandos**
<dl>
<dt>`--scope SCOPE`</dt>
<dd>Para configurar sua conta como o escopo, use `"*"` como o valor. Para configurar um namespace, um repositório ou uma tag como o escopo, insira o valor em um dos formatos a seguir: `namespace`, `namespace/repository`, `namespace/repository:tag`
</dd>
<dt>`--issue-type ISSUE_TYPE`</dt>
<dd>O tipo de problema de segurança que você deseja isentar. Para localizar tipos de problemas válidos, execute `ibmcloud cr exemption-types`.
</dd>
<dt>`--issue-id ISSUE_ID`</dt>
<dd>O ID do problema de segurança que você deseja isentar. Para localizar um ID de problema, execute `ibmcloud cr va <image>`, em que *&lt;image&gt;* é o nome de sua imagem, e use o valor relevante da coluna **ID de vulnerabilidade** ou **ID do problema de configuração**.
</dd>
</dl>

**Exemplos**

Crie uma isenção de CVE para o CVE com o ID `CVE-2018-17929` para todas as imagens no repositório `registry.ng.bluemix.net/birds/bluebird`.

```
ibmcloud cr exemption-add --scope registry.ng.bluemix.net/birds/bluebird --issue-type cve --issue-id CVE-2018-17929
```
{: pre}

Crie uma isenção CVE de toda a conta para CVE com o ID `CVE-2018-17929`.

```
ibmcloud cr exemption-add --scope "*" --issue-type cve --issue-id CVE-2018-17929
```
{: pre}

Crie uma isenção de problema de configuração para o problema `application_configuration:nginx.ssl_protocols` para uma única imagem com a tag `registry.ng.bluemix.net/birds/bluebird:1`.

```
ibmcloud cr exemption-add --scope registry.ng.bluemix.net/birds/bluebird:1 --issue-type configuration --issue-id application_configuration:nginx.ssl_protocols
```
{: pre}

## `ibmcloud cr exemption-list` (`ibmcloud cr exemptions`)
{: #bx_cr_exemption_list}

Liste suas isenções para problemas de segurança.

```
ibmcloud cr UNK-list [ -- scope SCOPE ]
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de acesso para configurar o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_configure).

**Opções de comandos**
<dl>
<dt>`--scope SCOPE`</dt>
<dd>(Opcional) Liste somente as isenções que se aplicarem a esse escopo. Para configurar um namespace, um repositório ou uma tag como o escopo, insira o valor em um dos formatos a seguir: `namespace`, `namespace/repository`, `namespace/repository:tag`
</dd>
</dl>

**Exemplo**

Liste todas as suas isenções para problemas de segurança que se aplicam a imagens no repositório *`birds/bluebird`*. A saída inclui isenções que são em toda a conta, isenções com escopo definido para o namespace *`birds`* e isenções com escopo definido para o repositório *`birds/bluebird`*, mas nenhuma com escopo definido para tags específicas dentro do repositório *`birds/bluebird`*.

```
ibmcloud cr exemption-list --scope birds/bluebird
```
{: pre}

## ` ibmcloud cr isenção-rm `
{: #bx_cr_exemption_rm}

Exclua uma isenção para um problema de segurança. Para visualizar suas isenções existentes, execute `ibmcloud cr exemption-list`.

```
ibmcloud cr exemption-rm --scope SCOPE --issue-type ISSUE_TYPE --issue-id ISSUE_ID
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de acesso para configurar o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_configure).

**Opções de comandos**
<dl>
<dt>`--scope SCOPE`</dt>
<dd>Para configurar sua conta como o escopo, use `"*"` como o valor. Para configurar um namespace, um repositório ou uma tag como o escopo, insira o valor em um dos formatos a seguir: `namespace`, `namespace/repository`, `namespace/repository:tag`
</dd>
<dt>`--issue-type ISSUE_TYPE`</dt>
<dd>O tipo de problema da isenção para o problema de segurança que você deseja remover. Para localizar os tipos de problemas para suas isenções, execute `ibmcloud cr exemption-list`.
</dd>
<dt>`--issue-id ISSUE_ID`</dt>
<dd>O ID da isenção para o problema de segurança que você deseja remover. Para localizar os IDs de problema para suas isenções, execute `ibmcloud cr exemption-list`.
</dd>
</dl>

**Exemplos**

Exclua uma isenção de CVE para o CVE com o ID `CVE-2018-17929` para todas as imagens no repositório `registry.ng.bluemix.net/birds/bluebird`.

```
ibmcloud cr exemption-rm --scope registry.ng.bluemix.net/birds/bluebird --issue-type cve --issue-id CVE-2018-17929
```
{: pre}

Exclua uma isenção CVE de toda a conta para CVE com o ID `CVE-2018-17929`.

```
ibmcloud cr exemption-rm --scope "*" --issue-type cve --issue-id CVE-2018-17929
```
{: pre}

Exclua uma isenção de problema de configuração para o problema `application_configuration:nginx.ssl_protocols` para uma única imagem com a tag `registry.ng.bluemix.net/birds/bluebird:1`.

```
ibmcloud cr exemption-rm --scope registry.ng.bluemix.net/birds/bluebird:1 --issue-type configuration --issue-id application_configuration:nginx.ssl_protocols
```
{: pre}

## `ibmcloud cr exemption-types
`
{: #bx_cr_exemption_types}

Lista os tipos de problemas de segurança que podem ser isentados.

```
ibmcloud cr exemption-types
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de acesso para configurar o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_configure).

## `ibmcloud cr iam-policies-enable`
{: #bx_cr_iam_policies_enable}

Se você estiver usando a autenticação do IAM, esse comando permitirá a autorização de baixa granularidade. Para obter mais informações, consulte [Gerenciando acesso de usuário com o Identity and Access Management](/docs/services/Registry/iam.html#iam) e [Definindo políticas de função de acesso do usuário](/docs/services/Registry/registry_users.html#user).

```
ibmcloud cr iam-policies-enable
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de acesso para configurar o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_configure).

## ` ibmcloud cr image-inspect `
{: #bx_cr_image_inspect}

Exibe detalhes sobre uma imagem específica.

```
ibmcloud cr image-inspect [--format FORMAT] IMAGE [IMAGE...]
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de acesso para usar o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using).

**Opções de comandos**
<dl>
<dt>`-- format FORMAT`</dt>
<dd>(Opcional) Formata os elementos de saída usando um modelo Go.

Para obter informações adicionais, consulte [Formatando e filtrando a saída de CLI para comandos do {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/registry_cli_reference.html#registry_cli_listing).

</dd>
<dt>`IMAGE`</dt>
<dd>O nome da imagem para o qual deseja obter um relatório. É possível inspecionar múltiplas imagens listando cada imagem no comando com um espaço entre cada nome.

<p>Para localizar os nomes de suas imagens, execute `ibmcloud cr image-list`. Combine o conteúdo das colunas **Repositório** e **Tag** para criar o nome da imagem no formato `repository:tag`. Se uma tag não for especificada no nome da imagem, a imagem identificada como `latest` será inspecionada. </p>

</dd>
</dl>

**Exemplo**

Exiba os detalhes sobre as portas expostas para a imagem *`registry.ng.bluemix.net/birds/bluebird:1`* usando a diretiva de formatação *`"{{ .Config.ExposedPorts }}"`*.

```
ibmcloud cr image-inspect  --format "{{ .Config.ExposedPorts }}" registry.ng.bluemix.net/birds/bluebird:1
```
{: pre}

## `ibmcloud cr image-list` (`ibmcloud cr images`)
{: #bx_cr_image_list}

Exibe todas as imagens na conta do {{site.data.keyword.Bluemix_notm}}.

O nome da imagem é a combinação do conteúdo das colunas **Repositório** e **Tag** no formato: `repository:tag`
{:tip}

```
ibmcloud cr image-list [--no-trunc] [--format FORMAT] [-q, --quiet] [--restrict RESTRICTION] [--include-ibm]
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de acesso para usar o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using).

**Opções de comandos**
<dl>
<dt>`--no-trunc`</dt>
<dd>(Opcional) Não truncar as compilações de imagem.</dd>
<dt>`-- format FORMAT`</dt>
<dd>(Opcional) Formata os elementos de saída usando um modelo Go.

Para obter informações adicionais, consulte [Formatando e filtrando a saída de CLI para comandos do {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/registry_cli_reference.html#registry_cli_listing).

</dd>
<dt>`-q`, `--quiet`</dt>
<dd>(Opcional) Cada imagem é listada no formato: `repository:tag`</dd>
<dt>`-- restrict RESTRICTION`</dt>
<dd>(Opcional) Limite a saída para exibir somente imagens no namespace especificado ou namespace e repositório. </dd>
<dt>`--include-ibm`</dt>
<dd>(Opcional) Inclui imagens públicas fornecidas pelo {{site.data.keyword.IBM_notm}} na saída. Sem essa opção, somente imagens privadas serão listadas, por padrão.</dd>
</dl>

**Exemplo**

Exiba as imagens no namespace *`birds`* no formato `repository:tag`, sem truncar as compilações de imagem.

```
ibmcloud cr image-list --restrict birds --quiet --no-trunc
```
{: pre}

## ` Imagem do ibmcloud cr-rm `
{: #bx_cr_image_rm}

Exclua uma ou mais imagens especificadas do {{site.data.keyword.registrylong_notm}}.

```
ibmcloud cr image-rm IMAGE [ IMAGE ...]
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de acesso para usar o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using).

**Opções de comandos**

<dl>
<dt>`IMAGE`</dt>
<dd>O nome da imagem que você deseja excluir. É possível excluir múltiplas imagens ao mesmo tempo listando cada imagem no comando com um espaço entre cada nome. `IMAGE` deve estar no formato `repository:tag`, por exemplo: `registry.ng.bluemix.net/namespace/image:latest`

<p>Para localizar os nomes de suas imagens, execute `ibmcloud cr image-list`. Combine o conteúdo das colunas **Repositório** e **Tag** para criar o nome da imagem no formato `repository:tag`. Se uma tag não for especificada no nome da imagem, a imagem identificada como `latest` será excluída por padrão.</p>

</dd>
</dl>

**Exemplo**
Exclua a imagem *`registry.ng.bluemix.net/birds/bluebird:1`*.

```
ibmcloud cr image-rm registry.ng.bluemix.net/birds/bluebird:1
```
{: pre}

## `ibmcloud cr image-tag`
{: #bx_cr_image_tag}

Crie uma nova imagem, TARGET_IMAGE, que se refira a uma imagem de origem, SOURCE_IMAGE, no {{site.data.keyword.registrylong_notm}}. As imagens de origem e de destino devem estar na mesma região.

Para localizar os nomes de suas imagens, execute `ibmcloud cr image-list`. Combine o conteúdo das colunas **Repositório** e **Tag** para criar o nome da imagem no formato `repository:tag`.
{: tip}

```
ibmcloud cr image-tag [SOURCE_IMAGE] [TARGET_IMAGE]
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de acesso para usar o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using).

**Opções de comandos**
<dl>
<dt>`SOURCE_IMAGE`</dt>
<dd>O nome da imagem de origem. `SOURCE_IMAGE` deve estar no formato `repository:tag`, por exemplo: `registry.ng.bluemix.net/namespace/image:latest`
</dd>
<dt>`TARGET_IMAGE`</dt>
<dd>O nome da imagem de destino. `TARGET_IMAGE` deve estar no formato `repository:tag`, por exemplo: `registry.ng.bluemix.net/namespace/image:latest`
</dd>
</dl>

**Exemplos**

Inclua outra referência de tag, `latest`, na imagem *`registry.ng.bluemix.net/birds/bluebird:1`*.

```
ibmcloud cr image-tag  registry.ng.bluemix.net/birds/bluebird:1 registry.ng.bluemix.net/birds/bluebird:latest
```
{: pre}

Copie a imagem `registry.ng.bluemix.net/birds/bluebird:peck` em outro repositório no mesmo namespace `birds/pigeon`.

```
ibmcloud cr image-tag registry.ng.bluemix.net/birds/bluebird:peck registry.ng.bluemix.net/birds/pigeon:peck
```
{: pre}

Copie a imagem `registry.ng.bluemix.net/birds/bluebird:peck` em outro namespace `animals` ao qual você tem acesso.

```
ibmcloud cr image-tag registry.ng.bluemix.net/birds/bluebird:peck registry.ng.bluemix.net/animals/dog:bark
```
{: pre}

## `ibmcloud cr info
`
{: #bx_cr_info}

Exibe o nome e a conta do registro ao qual você está conectado.

```
ibmcloud cr info
```
{: codeblock}

**Pré-requisitos**

Nenhum

## `  ibmcloud cr login
  `
{: #bx_cr_login}

Esse comando executa o comando `docker login` no registro. O comando `docker login` é necessário para poder executar os comandos `docker push` ou `docker pull` para o registro. Esse comando não é necessário para executar outros comandos `ibmcloud cr`. Se o Docker não estiver instalado, esse comando retornará uma mensagem de erro.

```
ibmcloud cr login
```
{: codeblock}

**Pré-requisitos**

Nenhum

## `ibmcloud cr namespace-add`
{: #bx_cr_namespace_add}

Escolha um nome para seu namespace e inclua-o em sua conta {{site.data.keyword.Bluemix_notm}}.

```
ibmcloud cr namespace-add NAMESPACE
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de acesso para usar o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using).

**Opções de comandos**
<dl>
<dt>`NAMESPACE`</dt>
<dd>O namespace que deseja incluir. O namespace deve ser exclusivo em todas as contas do {{site.data.keyword.Bluemix_notm}} na mesma região. Os namespaces devem ter de 4 a 30 caracteres e conter somente letra minúsculas, números, hifens e sublinhados. Os namespaces devem iniciar e terminar com uma letra ou número.
  
<p>  
<strong>Dica:</strong> não coloque informações pessoais em seus nomes de namespace.
</p>
  
</dd>
</dl>

**Exemplo**

Crie um namespace com o nome *`birds`*.

```
ibmcloud cr namespace-add birds
```
{: pre}

## `ibmcloud cr namespace-list` (`ibmcloud cr namespaces`)
{: #bx_cr_namespace_list}

Exibe todos os namespaces pertencentes à sua conta do {{site.data.keyword.Bluemix_notm}}.

```
ibmcloud cr namespace-list
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de acesso para usar o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using).

## ` ibmcloud cr namespace-rm `
{: #bx_cr_namespace_rm}

Remove um namespace de sua conta do {{site.data.keyword.Bluemix_notm}}. As imagens nesse namespace são excluídas quando o namespace é removido.

```
ibmcloud cr namespace-rm NAMESPACE
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de acesso para usar o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using).

**Opções de comandos**
<dl>
<dt>`NAMESPACE`</dt>
<dd>O namespace que deseja remover.</dd>
</dl>

**Exemplo**

Remova o namespace *`birds`*.

```
ibmcloud cr namespace-rm birds
```
{: pre}

## `ibmcloud cr plan
`
{: #bx_cr_plan}

Exibe seu plano de precificação.

```
ibmcloud cr plan
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de acesso para configurar o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_configure).

## ` ibmcloud cr plan-upgrade `
{: #bx_cr_plan_upgrade}

Faz upgrade para o plano padrão.

Para obter informações sobre planos, veja [Planos de registro](/docs/services/Registry/registry_overview.html#registry_plans).

```
ibmcloud cr plan-upgrade [PLAN]
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de acesso para configurar o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_configure).

**Opções de comandos**
<dl>
<dt>`PLANO`</dt>
<dd>(Opcional) O nome do plano de precificação para o qual você deseja fazer upgrade. Se `PLAN` não for especificado, o
padrão será `standard`.</dd>
</dl>

**Exemplo**

Faça upgrade para o plano de precificação padrão.

```
ibmcloud cr plan-upgrade standard
```
{: pre}

## ` ibmcloud cr ppa-archive-load `
{: #bx_cr_ppa_archive_load}

Importa o software {{site.data.keyword.IBM_notm}} que é transferido por download do [IBM Passport Advantage Online para clientes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/software/passportadvantage/pao_customer.html) e compactado para uso com o Helm em seu namespace do {{site.data.keyword.registrylong_notm}}.

As imagens de contêiner são enviadas por push para o namespace privado do {{site.data.keyword.registryshort_notm}}. Os gráficos de Helm são gravados em um diretório `ppa-import` criado no diretório em que o comando é executado. Opcionalmente, é possível usar o [Projeto de software livre Chart Museum ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/charts/tree/master/stable/chartmuseum) para hospedar gráficos de Helm.

```
ibmcloud cr ppa-archive-load --archive FILE --namespace NAMESPACE
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de acesso para usar o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using).

**Opções de comandos**
<dl>
  <dt>`-- archive FILE`</dt>
  <dd>O caminho para o arquivo compactado que é transferido por download por meio do IBM Passport Advantage.</dd>
  <dt>`-- namespace NAMESPACE`</dt>
  <dd>Um dos seus namespaces. Imagens do contêiner do arquivo compactado são enviadas por push para esse namespace. Para listar namespaces, execute `ibmcloud cr namespace-list`.</dd>
  <dt>`--chartmuseum-uri URI`</dt>
  <dd>(Opcional) Seu identificador de recurso exclusivo do [Chart Museum ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/charts/tree/master/stable/chartmuseum).</dd>
  <dt>`-- USUÁRIO chartmuseum Usuário`</dt>
  <dd>(Opcional) Seu nome de usuário do [Chart Museum ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/charts/tree/master/stable/chartmuseum).</dd>
  <dt>`-- chartmuseum-password PASSWORD`</dt>
  <dd>(Opcional) Sua senha do [Chart Museum ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/charts/tree/master/stable/chartmuseum).</dd>
</dl>

**Exemplo**

Importe o software IBM e compacte-o para uso com o Helm em seu namespace *`birds`* do {{site.data.keyword.registrylong_notm}}, em que o caminho para o arquivo compactado é *`downloads/compressed_file.tgz`*.

```
ibmcloud cr ppa-archive-load --archive downloads/compressed_file.tgz --namespace birds
```
{: pre}

## `    ibmcloud cr quota
    `
{: #bx_cr_quota}

Exibe suas cotas atuais para tráfego e armazenamento e informações de uso com relação a essas cotas.

```
ibmcloud cr quota
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de acesso para configurar o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_configure).

## ` cota ibmcloud cr-set `
{: #bx_cr_quota_set}

Modifique a cota especificada.

```
ibmcloud cr quota-set [--traffic TRAFFIC] [--storage STORAGE]
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de acesso para configurar o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_configure).

**Opções de comandos**
<dl>
<dt>`-- traffic TRAFFIC`</dt>
<dd>(Opcional) Muda sua cota de tráfego para o valor especificado em megabytes. A operação falhará se você não estiver autorizado a configurar o tráfego ou se configurar um valor que exceda o seu plano de precificação atual.</dd>
<dt>`-- storage STORAGE`</dt>
<dd>(Opcional) Muda sua cota de armazenamento para o valor especificado em megabytes. A operação falhará se você não estiver autorizado a configurar cotas de armazenamento ou se configurar um valor que exceda o seu plano de precificação atual.</dd>
</dl>

**Exemplo**

Configure o limite de cota para o tráfego de pull como *7000* megabytes e para o armazenamento como *600* megabytes.

```
ibmcloud cr quota-set --traffic 7000 --storage 600
```
{: pre}

## `ibmcloud cr region
`
{: #bx_cr_region}

Exibe a região de destino e o registro.

```
ibmcloud cr region
```
{: codeblock}

**Pré-requisitos**

Nenhum

Para obter mais informações, consulte [Regiões](/docs/services/Registry/registry_overview.html#registry_regions).

## ` ibmcloud cr region-set `
{: #bx_cr_region_set}

Configure uma região de destino para os comandos do {{site.data.keyword.registrylong_notm}}. Para listar as regiões disponíveis, execute o comando sem parâmetros.

```
ibmcloud cr region-set [ REGION ]
```
{: codeblock}

**Pré-requisitos**

Nenhum

**Opções de comandos**
<dl>
<dt>`REGION`</dt>
<dd>(Opcional) O nome de sua região de destino, por exemplo, `us-south`.

Para obter mais informações, consulte [Regiões](/docs/services/Registry/registry_overview.html#registry_regions).

</dd>
</dl>

**Exemplo**

Tenha como destino a região Sul dos EUA.

```
ibmcloud cr region-set us-south
```
{: pre}

## ` ibmcloud cr token-add `
{: #bx_cr_token_add}

Inclua um token que possa ser usado para controlar o acesso a um registro.

```
ibmcloud cr token-add [--description DESCRIPTION] [-q, --quiet] [--non-expiring] [--readwrite]
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de gerenciamento de plataforma](/docs/services/Registry/iam.html#platform_management_roles).

**Opções de comandos**
<dl>
<dt>`--description DESCRIPTION`</dt>
<dd>(Opcional) Especifica o valor como uma descrição para o token, que é exibido quando você executa `ibmcloud cr token-list`. Se o seu token for criado automaticamente pelo {{site.data.keyword.containerlong_notm}}, a descrição será configurada para o nome do cluster do Kubernetes. Nesse caso, o token é removido automaticamente quando o cluster é removido.
  
<p> 
  <strong>Dica:</strong> não coloque informações pessoais na descrição do seu token.
</p>

</dd>
<dt>`-q`, `--quiet`</dt>
<dd>(Opcional) Exibe o token somente, sem nenhum texto circundante.</dd>
<dt>`--non-expiring`</dt>
<dd>(Opcional) Cria um token com acesso que não expira. Sem esse conjunto de parâmetros, o acesso por meio de um token expirará após 24 horas, por padrão.</dd>
<dt>`--readwrite`</dt>
<dd>(Opcional) Cria um token que concede acesso de leitura e gravação. Sem essa opção, o acesso será somente leitura, por padrão.</dd>
</dl>

**Exemplo**

Inclua um token com a descrição *Token para minha conta* que não expire e tenha acesso de leitura/gravação.

```
ibmcloud cr token-add --description "Token for my account" --non-expiring --readwrite
```
{: pre}

## `ibmcloud cr token-get`
{: #bx_cr_token_get}

Recuperar o token especificado do registro.

```
ibmcloud cr token-get TOKEN
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de gerenciamento de plataforma](/docs/services/Registry/iam.html#platform_management_roles).

**Opções de comandos**
<dl>
<dt>`TOKEN`</dt>
<dd>O identificador exclusivo do token que você deseja recuperar. Para listar seus tokens, execute `ibmcloud cr token-list`.</dd>
</dl>

**Exemplo**

Recupere o token *10101010-101x-1x10-x1xx-x10xx10xxx10*.

```
ibmcloud cr token-get 10101010-101x-1x10-x1xx-x10xx10xxx10
```
{: pre}

## `ibmcloud cr token-list` (`ibmcloud cr tokens`)
{: #bx_cr_token_list}

Exibe todos os tokens que existem para sua conta do {{site.data.keyword.Bluemix_notm}}.

```
ibmcloud cr token-list [--format FORMAT]
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de gerenciamento de plataforma](/docs/services/Registry/iam.html#platform_management_roles).

**Opções de comandos**
<dl>
<dt>`-- format FORMAT`</dt>
<dd>(Opcional) Formata os elementos de saída usando um modelo Go.

Para obter informações adicionais, consulte [Formatando e filtrando a saída de CLI para comandos do {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/registry_cli_reference.html#registry_cli_listing).

</dd>
</dl>

**Exemplo**

Exiba todos os tokens que sejam somente leitura, usando a diretiva de formatação *`"{{ if eq .ReadOnly true}}{{.ID}} - {{.Expiry}} - {{.ReadOnly}} - {{.Description}}{{ end }}"`*.

```
ibmcloud cr token-list --format "{{ if eq .ReadOnly true}}{{.ID}} - {{.Expiry}} - {{.ReadOnly}} - {{.Description}}{{ end }}"
```
{: pre}

Este exemplo produz saída no formato a seguir:

```
10101010-101a-1a10-a1aa-a10aa10aaa10 - 1234567890 - true - demo
```
{: screen}

## ` ibmcloud cr token-rm `
{: #bx_cr_token_rm}

Remover um ou mais tokens especificados.

```
ibmcloud cr token-rm TOKEN [ TOKEN ...]
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de gerenciamento de plataforma](/docs/services/Registry/iam.html#platform_management_roles).

**Opções de comandos**
<dl>
<dt>`TOKEN`</dt>
<dd>O TOKEN pode ser o próprio token ou o identificador exclusivo do token, conforme mostrado em `ibmcloud cr token-list`. É possível especificar múltiplos tokens e devem ser separados por um espaço.</dd>
</dl>

**Exemplo**

Remova o token *10101010-101x-1x10-x1xx-x10xx10xxx10*.

```
ibmcloud cr token-rm 10101010-101x-1x10-x1xx-x10xx10xxx10
```
{: pre}

## `ibmcloud cr vulnerability-assessment` (`ibmcloud cr va`)
{: #bx_cr_va}

Visualize um relatório de avaliação de vulnerabilidade para suas imagens.

```
ibmcloud cr vulnerability-assessment [--extended | -e] [--vulnerabilities | -v] [--configuration-issues | -c] [--output FORMAT | -o FORMAT] IMAGE [IMAGE...]
```
{: codeblock}

**Pré-requisitos**

Para saber sobre as permissões necessárias, consulte [Funções de acesso para usar o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry/iam.html#access_roles_using).

**Opções de comandos**
<dl>
<dt>`IMAGE`</dt>
<dd>O nome da imagem para o qual deseja obter um relatório. O relatório indica se a imagem em quaisquer vulnerabilidades de pacote conhecidas. É possível solicitar relatórios para múltiplas imagens ao mesmo tempo listando cada imagem no comando com um espaço entre cada nome.

<p>Para localizar os nomes de suas imagens, execute `ibmcloud cr image-list`. Combine o conteúdo das colunas **Repositório** e **Tag** para criar o nome da imagem no formato `repository:tag`. Se uma tag não for especificada no nome da imagem, o relatório avaliará a imagem identificada como `latest`.</p>

<p>Os seguintes sistemas operacionais são suportados:

<ul>
<li>Alpine</li>
<li>CentOS</li>
<li>Debian</li>
<li>Red Hat Enterprise Linux (RHEL)</li>
<li>Ubuntu</li>
</ul>
</p>

Para obter mais informações, consulte [Gerenciando a segurança de imagens com o Vulnerability Advisor](/docs/services/va/va_index.html).

</dd>
<dt>`--output FORMAT`, `-o FORMAT`</dt>
<dd>(Opcional) A saída de comando é retornada no formato escolhido. O formato padrão é `text`. Os seguintes formatos são suportados:

<ul>
<li>`text`</li>
<li>`json`</li>
</ul>

</dd>
<dt>`--vulnerabilities`, `-v`</dt>
<dd>(Opcional) A saída de comando é restrita para mostrar somente as vulnerabilidades.</dd>
<dt>`--configuration-issues`, `-c`</dt>
<dd>(Opcional) A saída de comando é restrita para mostrar somente problemas de configuração.</dd>
<dt>`--extended`, `-e`</dt>
<dd>(Opcional) A saída de comando mostra informações adicionais sobre correções para pacotes vulneráveis.</dd>

</dl>

**Exemplos**

Visualize um relatório de avaliação de vulnerabilidade padrão para sua imagem.

```
ibmcloud cr vulnerability-assessment registry.ng.bluemix.net/birds/bluebird:1
```
{: pre}

Visualize um relatório de avaliação de vulnerabilidade para sua imagem *`registry.ng.bluemix.net/birds/bluebird:1`* em formato JSON, mostrando somente vulnerabilidades.

```
ibmcloud cr vulnerability-assessment --vulnerabilities  --output json registry.ng.bluemix.net/birds/bluebird:1
```
{: pre}
