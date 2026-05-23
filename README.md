# 🐧 Guia de Configuração do Swappiness no Linux

## 📖 O que é SWAP?
O **espaço de SWAP** é uma partição ou arquivo localizado em uma mídia de armazenamento permanente (HD, SSD ou NVMe) que atua como uma **extensão da memória RAM** do sistema.

### 🧠 Como funciona na prática?
Quando o sistema operacional identifica uma das seguintes situações:

- ✅ **Baixa disponibilidade de memória RAM**
- ✅ **Porções da memória permanecem inalteradas por um período**

Ele move dados da RAM "real" para o espaço de SWAP, liberando memória física para processos mais ativos ou prioritários.

```mermaid
flowchart LR
    A[Processos Ativos] --> B[🐏 RAM]
    B -->|"🔁 Dados inativos"| C[💾 SWAP]
    C -->|"📤 Dados necessários novamente"| B
 ```

## 🎯 O que é Swappiness?
Swappiness é um parâmetro do kernel Linux (presente desde a versão 2.6) que define a "vontade" do sistema em usar a SWAP.

## 📊 Como interpretar o valor?
```mermaid
graph LR
    A[0] -->|"Evita SWAP"| B[60]
    B -->|"Equilíbrio"| C[100]
    C -->|"Usa SWAP agressivamente"| D[100]
```

</br>

## 🎯 Qual é o melhor valor de swappiness?
<p styele="text-align: justify;">A escolha do melhor valor de swappiness depende de diversos fatores diferentes, incluindo: </p>

- ⚙️ **Configuração do sistema** (hardware, kernel, drivers)
- 📊 **Carga de trabalho** (tipo de aplicações executadas)
- 🧠 **Memória RAM disponível** (total e uso típico)
- ⚡ **Requisitos de desempenho** (latência vs. throughput)
---
> 💡 **Princípio básico:** Quanto menor o valor de swappiness, menos o sistema utiliza a swap, priorizando a memória RAM. Quanto maior o valor, mais agressivo o sistema será ao mover dados para a swap.

--- 

### 📊 Tabela 1: Valores de swappiness e seus efeitos

<table style="width: 100%; border-collapse: collapse;">
  <thead>
    <tr style="background-color: #666666; color: white;">
      <th style="padding: 12px; border: 1px solid #999;">Valor</th>
      <th style="padding: 12px; border: 1px solid #999;">Efeito</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background-color: #e3f2ed;"> <!-- cinza claro -->
      <td style="padding: 10px; border: 1px solid #999; color: #0f0202">0</td>
      <td style="padding: 10px; border: 1px solid #999; color: #0f0202"> O kernel evita a troca pelo maior tempo possível, só utilizando swap quando absolutamente necessário.</td>
    </tr>
    <tr style="background-color: #a0a0a0; color: white;"> <!-- cinza escuro -->
      <td style="padding: 10px; border: 1px solid #999;">10-30</td>
      <td style="padding: 10px; border: 1px solid #999;">	Troca leve. Recomendado para desktops e estações de trabalho onde a capacidade de resposta é prioridade.</td>
    </tr>
    <tr style="background-color: #f0f0f0;">
      <td style="padding: 10px; border: 1px solid #999; color: #0f0202;">60</td>
      <td style="padding: 10px; border: 1px solid #999;color: #0f0202;">Valor padrão na maioria das distribuições Linux. Adequado para sistemas de uso geral.</td>
    </tr>
    <tr style="background-color: #a0a0a0; color: white;"> <!-- cinza escuro -->
      <td style="padding: 10px; border: 1px solid #999;">80-100</td>
      <td style="padding: 10px; border: 1px solid #999;">Troca agressiva. Indicado para sistemas onde a taxa de transferência da swap é mais importante que a latência.</td>
    </tr>
  </tbody>
</table>

---
> ⚠️ **Atenção:** Acessar memória swap é **muito mais lento** (até 1000x) do que acessar memória RAM física. Portanto, valores de swappiness mais baixos geralmente **melhoram a capacidade de resposta** do sistema em desktops e notebooks.

</br>

### 🖥️ Tabela 2: Recomendações por tipo de carga de trabalho

<table style="width: 100%; border-collapse: collapse;">
  <thead>
    <tr style="background-color: #666666; color: white;">
      <th style="padding: 12px; border: 1px solid #999;">Carga de Trabalho</th>
      <th style="padding: 12px; border: 1px solid #999;">Valor Recomendado</th>
      <th style="padding: 12px; border: 1px solid #999;">Justificativa</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background-color: #e3f2ed;"> <!-- cinza claro -->
      <td style="padding: 10px; border: 1px solid #999; color: #0f0202">Desktop / Estação de trabalho</td>
      <td style="padding: 10px; border: 1px solid #999; color: #0f0202"> 10-15</td>
      <td style="padding: 10px; border: 1px solid #999; color: #0f0202">Prioriza responsividade e mantém aplicações ativas na RAM.</td>
    </tr>
    <tr style="background-color: #a0a0a0; color: white;"> <!-- cinza escuro -->
      <td style="padding: 10px; border: 1px solid #999;"> Servidor em geral</td>
      <td style="padding: 10px; border: 1px solid #999;">30-60</td>
      <td style="padding: 10px; border: 1px solid #999;">Equilíbrio entre desempenho e disponibilidade de memória</td>
    </tr>
    <tr style="background-color: #f0f0f0;">
      <td style="padding: 10px; border: 1px solid #999; color: #0f0202;"> Servidor de banco de dados (MySQL, PostgresSQL)</td>
      <td style="padding: 10px; border: 1px solid #999;color: #0f0202;">1-10</td>
      <td style="padding: 10px; border: 1px solid #999;color: #0f0202;">Banco de dados se beneficiam enormemente de manter dados na RAM.</td>
    </tr>
    <tr style="background-color: #a0a0a0; color: white;"> <!-- cinza escuro -->
      <td style="padding: 10px; border: 1px solid #999;">Sistemas com NVMe rápido</td>
      <td style="padding: 10px; border: 1px solid #999;">20-40</td>
      <td style="padding: 10px; border: 1px solid #999;">Swap em NVMe é mais rápida, permitindo valores um mais altos sem grande perda.</td>
    </tr>
  </tbody>
</table>

---

> 📌 Observação final: O valor ideal depende da sua carga de trabalho e da RAM disponível. Ajuste em pequenos incrementos e monitore o desempenho após cada alteração.

</br>

## 🔍 Como verificar o valor atual do swappiness
Antes de realizar qualquer alteração, é importante saber qual é o valor atual configurado no seu sistema. Existem duas formas simples e equivalentes de fazer essa verificação.

### 📁 Opção 1: Lendo diretamente do arquivo do sistema
O kernel do Linux expõe o valor do swappiness através de um arquivo virtual no diretório `/proc/sys/vm/`. Você pode ler seu conteúdo com o comando `cat`:

```bash
alan@linux:~$ cat /proc/sys/vm/swappiness
60
```
---
### 🛠️ Opção 2: Usando o comando ```sysctl```
O comando sysctl foi criado especificamente para ler e modificar parâmetros do kernel de forma mais amigável:

```bash
alan@linux:~$ sysctl vm.swappiness
vm.swappiness = 10
```

</br>

## ⚙️ Como alterar o valor de swappiness temporariamente
Para testar um novo valor sem torná-lo permanente, utilize o comando sysctl com privilégios de superusuário:

```bash
alan@linux:~$ sudo sysctl vm.swappiness=25
vm.swappiness = 25
```
⚠️ Importante: Esta alteração é temporária e será perdida após a reinicialização do sistema. Se o novo valor atender às suas necessidades, consulte a seção a seguir para persistir a configuração.

</br>

## 💾 Como tornar a alteração permanente
Para que o valor de `swappiness` seja mantido após reinicializações, você precisa persistir a configuração. Existem duas abordagens recomendadas, sendo a primeira a mais moderna e preferível.

### 📁 Opção 1: Arquivo drop-in dedicado (recomendado)
Esta é a abordagem mais limpa e alinhada com as práticas atuais do Linux. Em vez de modificar arquivos do sistema diretamente, você cria um arquivo separado no diretório `/etc/sysctl.d/`.

**Passo 1:** Crie o arquivo de configuração:
```bash
sudo nano /etc/sysctl.d/99-swappiness.conf
```

**Passo 2:** Adicione a seguinte linha ao arquivo:
```bash
vm.swappiness=[value]
```

**Passo 3:** Salve o arquivo e aplique a alteração sem reiniciar:
```bash
sudo sysctl --system
```

--- 
### 📄 Opção 2: Arquivo ```/etc/sysctl.conf``` (tradicional)
Esta é a forma clássica, ainda funcional, mas menos organizada para configurações personalizadas.

**Passo 1:** Abra o arquivo de configuração principal:
```bash
sudo nano /etc/sysctl.conf
```
**Passo 2:** Acrescente a linha ao final do arquivo:
```bash
vm.swappiness=[value]
```
**Passo 3:** Salve o arquivo e aplique as alterações:
```bash
sudo systcl --system

```
</br>

---

### 📚 Referências
- 🔗 [PhoenixNAP - Swappiness](https://phoenixnap.com/kb/swappiness)
- 🔗 [Debian Wiki - Swappiness (PT)](https://wiki-debian-org.translate.goog/swappiness?_x_tr_sl=en&_x_tr_tl=pt&_x_tr_hl=pt&_x_tr_pto=tc)
- 🔗 [Linuxize - Swappiness](https://linuxize.com/post/how-to-change-the-swappiness-value-in-linux/)

### 👨‍💻 Autor
**Alan S. N.** | Graduado em Licenciatura em Computação - IFBA- Campus Porto Seguro  
[Conecte-se comigo no LinkedIn](https://www.linkedin.com/in/alan-sn/)