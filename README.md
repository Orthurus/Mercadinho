# Mercadinho

Sistema de caixa e estoque para mercado. Roda no Windows como um único `.exe`.

**Telas:** Vendas (leitor de código de barras) · Produtos (sugere o preço pela margem) · Estoque (sugestão de compra pelos mais vendidos) · Fiado (substitui o caderno) · Folha de Caixa (automática) · Cartões (informar o valor que realmente caiu, quando quiser) · Configurações.

## Rodar durante o desenvolvimento
```
python main.py
```
Não precisa instalar nenhuma biblioteca (só Python 3.10+ com Tkinter, que já vem no instalador do python.org).

## Gerar o .exe
Dê dois cliques em `build.bat` → o programa fica em `dist\Mercadinho.exe`.

## Publicar uma atualização
1. Altere o código e faça commit/push.
2. Crie a tag da nova versão:
   ```
   git tag v1.0.1
   git push origin v1.0.1
   ```
3. O GitHub Actions gera o `Mercadinho.exe` e publica na aba **Releases**.
4. Na próxima vez que o programa abrir (com internet), ele avisa e atualiza sozinho.

> A primeira versão precisa ser instalada na mão (copie o `Mercadinho.exe` para uma pasta como Documentos). As seguintes se atualizam sozinhas.

## Onde ficam os dados
`%APPDATA%\Mercadinho\` — banco (`mercadinho.db`), `backups\` (um por dia, guarda 30) e `fechamentos\` (folha de caixa de cada dia em .txt). Atualizar o programa nunca mexe nesses dados.

## Leitor de código de barras
Leitores USB funcionam como teclado: basta o cursor estar no campo de código e passar o produto.
