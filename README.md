# 🛒 Mercadinho

Sistema de caixa (PDV) e controle de estoque para mercado, feito em Python.
Roda no Windows como um único `.exe`, sem instalar nada, e se atualiza sozinho pelo GitHub.

## Funcionalidades

| Tela | O que faz |
|---|---|
| **Vendas** | Frente de caixa estilo PDV com leitor de código de barras, lista de itens, subtotal, total recebido e troco. Aceita dinheiro, débito, crédito, Pix e fiado (inclusive misturando formas na mesma venda). |
| **Produtos** | Cadastro por código de barras. Sugere o preço de venda a partir do custo + margem definida em Configurações. |
| **Estoque** | Baixa automática a cada venda, entrada de mercadoria e **sugestão de compra** pelos itens mais vendidos nos últimos 30 dias. Gera lista de compras. |
| **Fiado** | Substitui o caderno: saldo por cliente, compras anotadas e recebimentos. Venda no fiado feita no caixa já entra aqui. |
| **Folha de Caixa** | Gerada automaticamente: vendas por forma de pagamento, dinheiro esperado na gaveta, fiado do dia, lucro estimado e mais vendidos. Salva um arquivo `.txt` por dia. |
| **Cartões** | A maquininha cobra taxa, então o valor que realmente caiu na conta é informado **depois**, sem pressa: por venda ou pelo total do dia. Mostra a taxa real. |
| **Configurações** | Nome do mercado, margem de lucro, arredondamento do preço, dias de cobertura do estoque, taxas estimadas, backup e atualização. |

### Atalhos da tela de Vendas

| Tecla | Ação | Tecla | Ação |
|---|---|---|---|
| `F1` | Voltar às Vendas | `F8` | Consultar preço |
| `F2` | Pesquisar produto pelo nome | `F10` | **Finalizar venda** |
| `F3` | Excluir item | `F11` | Cancelar venda |
| `F4` | Quantidade | `Ctrl+R` | Fiado |
| `F5` | Nova venda | `Ctrl+P` / `Ctrl+E` | Produtos / Estoque |

Dica: digitar `3*código` no campo de código adiciona 3 unidades.

## Para usar (seu pai)

1. Baixe o `Mercadinho.exe` na aba **Releases** (versão mais recente).
2. Copie para uma pasta como *Documentos* e abra.
3. Pronto. Quando houver versão nova, o programa avisa e atualiza sozinho (precisa de internet).

> O Windows pode mostrar um aviso de segurança na primeira vez, porque o programa não tem assinatura digital. Clique em **Mais informações → Executar assim mesmo**.

## Para desenvolver

Requisitos: Python 3.10+ (instalador do python.org, que já inclui o Tkinter). Não há bibliotecas externas.

```
python main.py
```

Estrutura:

```
main.py      interface (Tkinter)
db.py        banco SQLite e regras (vendas, fiado, cartões, folha, estoque)
updater.py   atualização automática via GitHub Releases
version.py   número da versão
build.bat    gera o .exe localmente (PyInstaller)
.github/workflows/release.yml   gera o .exe e publica a Release ao criar uma tag
```

## Publicar uma atualização

1. Faça commit e push das mudanças.
2. Crie a tag da nova versão, sempre com três números (`v1.0.1`, `v1.0.2`, `v1.1.0`...):
   ```
   git tag v1.0.1
   git push origin v1.0.1
   ```
3. O GitHub Actions gera o `Mercadinho.exe` e o anexa à Release automaticamente.
4. Na próxima abertura, o programa do cliente detecta e atualiza.

Se o Actions der erro de permissão: *Settings → Actions → General → Workflow permissions → Read and write*.

Publicando manualmente: ajuste `version.py` para o mesmo número da tag antes de gerar o `.exe`, e anexe o arquivo com o nome exato `Mercadinho.exe`.

## Onde ficam os dados

`%APPDATA%\Mercadinho\`

- `mercadinho.db`: banco de dados
- `backups\`: um backup por dia (guarda os últimos 30)
- `fechamentos\`: folha de caixa de cada dia e listas de compras

Atualizar o programa nunca altera esses dados. Copie a pasta `backups` para um pendrive de vez em quando.

## Observações

- Leitores USB de código de barras funcionam como teclado: basta o cursor estar no campo de código.
- Não emite nota fiscal (NFC-e). Consulte o contador sobre a obrigatoriedade.
- As taxas de cartão em Configurações são só **estimativas** enquanto o valor real não é informado na tela Cartões.
