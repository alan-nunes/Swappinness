# Swappinness

## O que é?
<p style="text-align: justify;">
O espaço de SWAP é uma partição e/ou arquivo localizado em uma mídia de armazenamento permanente, como um disco rígido ou SSD, que é tratado como uma extensão da memória RAM do sistema. Quando o sistema está com pouca memória ou quando uma determinada porção da memória permanece inalterada por algum tempo, os dados podem ser movidos da memória RAM "real" para o espaço de SWAP até que sejam necessários novamente, liberando assim a memória RAM para processos mais ativos ou de maior prioridade. </p>

## Qual é o melhor valor de swappiness?
<p styele="text-align: justify;">A escolha do melhor valor de swappiness depende de muitos fatores diferentes, tais como: </p>

- Configuração do sistema.
- Carga de trabalho.
- Memória.
- Requisitos de desempenho.

### A tabela abaixo fornece exemplos de valores de swappiness e seus efeitos:

<table style="width: 100%; border-collapse: collapse;">
  <thead>
    <tr style="background-color: #666666; color: white;">
      <th style="padding: 12px; border: 1px solid #999;">Valor de Troca</th>
      <th style="padding: 12px; border: 1px solid #999;">Efeito da troca</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background-color: #e3f2ed;"> <!-- cinza claro -->
      <td style="padding: 10px; border: 1px solid #999; color: #0f0202">0</td>
      <td style="padding: 10px; border: 1px solid #999; color: #0f0202"> O valor instrui o kernel a evitar ao máximo a troca.</td>
    </tr>
    <tr style="background-color: #a0a0a0; color: white;"> <!-- cinza escuro -->
      <td style="padding: 0px; border: 1px solid #999;">10-50</td>
      <td style="padding: 10px; border: 1px solid #999;">	O valor informa ao kernel para ser um pouco agressivo na troca de páginas de memória.</td>
    </tr>
    <tr style="background-color: #f0f0f0;">
      <td style="padding: 10px; border: 1px solid #999; color: #0f0202;">50-100</td>
      <td style="padding: 10px; border: 1px solid #999;color: #0f0202;">O valor instrui o kernel a ser moderadamente agressivo na troca de páginas de memória.</td>
    </tr>
  </tbody>
</table>

## Como verificar o valor de swappiness

### Opção 1: Verificando o arquivo ```/proc/sys/vm/swappiness```
```bash
cat /proc/sys/vm/swappiness
```
