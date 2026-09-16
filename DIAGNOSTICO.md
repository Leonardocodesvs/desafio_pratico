# Diagnóstico do site Biogutex

## Introdução

Fiz alguns testes no site da Biogutex para procurar problemas de funcionamento e de responsividade.

Testei a página nas resoluções:

* 360px
* 768px
* 1440px

Também testei os botões de compra e olhei o Console do navegador para verificar se apareciam erros.

Abaixo estão os principais problemas que encontrei.

---

## 1. Erro no JavaScript dos links dos kits

### O que acontece

No Console apareceu este erro:

```text
Uncaught TypeError: Cannot read properties of null (reading 'split')
```

### Onde acontece

O erro aparece no arquivo `main.js`, na parte que pega os links dentro da área dos kits.

O código procura todos os links:

```js
let buttons = document.querySelectorAll(".area-kits a");
```

Depois pega o `href`:

```js
let originalHref = button.getAttribute("href");
```

Porém, um dos links não possui `href`:

```html
<a>
```

Nesse caso, o `getAttribute("href")` retorna `null`.

Depois o código tenta usar:

```js
originalHref.split("?")
```

e por isso acontece o erro.

### Como eu corrigiria

Eu colocaria uma verificação antes de usar o `href`:

```js
let originalHref = button.getAttribute("href");

if (!originalHref) {
    return;
}
```

Também verificaria os links dos três kits para garantir que todos estejam corretos.

### Severidade

**Crítica**

Porque existe um erro no JavaScript e ele está relacionado aos links da área de compra.

---

## 2. Botões de compra levam para uma página 404

### O que acontece

Testei os botões de compra dos kits e eles levam para uma página com erro **404 Not Found**.

### Onde acontece

Na seção dos kits:

```html
<section class="area-kits pd" id="kits">
```

Existem links como:

```html
<a href="linkoffer">
```

e:

```html
<a href="linkoffer3">
```

Também existe um `<a>` sem `href`.

### Como eu identifiquei

Cliquei nos botões de compra e a página mostrou uma mensagem de página não encontrada.

### Como eu corrigiria

Verificaria os links utilizados nos botões e colocaria o endereço correto da página de checkout.

Também colocaria um `href` válido no segundo kit.

### Severidade

**Crítica**

Porque o usuário não consegue continuar corretamente para realizar a compra.

---

## 3. Cards dos kits ficam maiores que a tela no celular

### O que acontece

No teste em **360px**, percebi que os cards dos kits ficam maiores que a largura da tela.

Por exemplo, o card `.kit-option.k1` ficou com aproximadamente:

```text
393,8px
```

enquanto a tela tinha:

```text
360px
```

### Onde acontece

O problema aparece na seção:

```html
<section class="area-kits pd" id="kits">
```

### Problema encontrado

Por causa disso, parte do conteúdo fica cortada e a página não se adapta corretamente à tela pequena.

### Como eu corrigiria

Eu verificaria os `width`, `min-width`, `padding` e os tamanhos das imagens dos cards.

Também poderia usar algo como:

```css
.kit-option {
    width: 100%;
    max-width: 100%;
    box-sizing: border-box;
}
```

Depois faria novos testes em celulares.

### Severidade

**Crítica**

Porque o problema aparece em uma tela pequena e pode dificultar bastante a utilização da página no celular.

---

## 4. Alguns textos da seção "Sobre" estão difíceis de ler

### O que acontece

No teste em **768px**, percebi que alguns textos da seção "Sobre" ficam com pouco contraste com o fundo.

### Problema encontrado

A cor do texto não fica muito fácil de enxergar.

### Como eu corrigiria

Eu mudaria a cor do texto para uma cor com mais contraste.

Por exemplo:

```css
color: white;
```

Também faria um teste de contraste para confirmar a melhor cor.

### Severidade

**Média**

Não impede o funcionamento da página, mas dificulta a leitura.

---

## 5. Número da avaliação está difícil de enxergar

### O que acontece

Na parte da avaliação do produto, o número que aparece junto das estrelas fica pouco visível.

### Problema encontrado

A cor do número não tem muito contraste com o fundo.

### Como eu corrigiria

Eu mudaria a cor do número para uma cor mais fácil de enxergar.

Por exemplo:

```css
color: white;
```

### Severidade

**Média**

É um problema visual e de leitura, mas não impede o usuário de usar a página.

---

## 6. Texto da seção "O que esperar" está com pouco contraste

### O que acontece

Na seção "O que esperar", alguns textos aparecem em vermelho e ficam difíceis de ler por causa do fundo.

### Problema encontrado

O contraste entre o texto e o fundo não está muito bom.

### Como eu corrigiria

Eu testaria outra cor para o texto.

Por exemplo:

```css
color: white;
```

Depois verificaria se a leitura melhorou.

### Severidade

**Média**

Porque dificulta a leitura, mas não impede o funcionamento da página.

---

## 7. Link "Contact Page" está difícil de enxergar

### O que acontece

Na seção de garantia existe o link:

```html
<a href="contact.hmtl" id="hyperlink-contact">
    link to our Contact Page
</a>
```

Durante o teste, percebi que o link fica pouco visível por causa da cor.

### Problema encontrado

O link aparece em vermelho e possui pouco contraste com o fundo.

Também verifiquei que o endereço está escrito como:

```text
contact.hmtl
```

Então eu também verificaria se esse nome está correto.

### Como eu corrigiria

Eu mudaria a cor do link e manteria o sublinhado para deixar claro que ele é clicável.

Exemplo:

```css
#hyperlink-contact {
    color: white;
    text-decoration: underline;
}
```

### Severidade

**Média**

Porque o link fica mais difícil de identificar e ler.

---

# Suspeita de bug

## 8. Página começa a rolar sozinha ao mudar a resolução

### O que aconteceu

Durante os testes de responsividade, percebi um comportamento estranho.

Quando aumentei ou alterei a resolução da página, em determinado momento ela começou a descer/rolar sozinha.

### Onde aconteceu

O problema foi percebido durante a alteração do tamanho da janela.

### Possível causa

Ainda não consegui descobrir exatamente o que está causando isso.

Pode estar relacionado a algum JavaScript, evento de `scroll` ou algum elemento sendo reposicionado quando o tamanho da tela muda.

### Como eu investigaria

Eu verificaria:

* os eventos de `scroll`;
* os códigos JavaScript executados durante o redimensionamento;
* se algum elemento está recebendo foco;
* se algum componente está mudando de posição;
* se o problema acontece em outros navegadores.

### Severidade

**Baixa — Suspeita**

Coloquei como suspeita porque consegui perceber o comportamento, mas ainda não confirmei exatamente a causa.

---

# Outras observações

Durante os testes, apareceram algumas mensagens no Console relacionadas a scripts externos, carregamento de imagens e bloqueio de recursos.

Não coloquei essas mensagens como bugs confirmados porque elas podem estar relacionadas ao navegador ou ao bloqueio de recursos durante o teste.

Também encontrei uma parte do código do FAQ que está comentada no JavaScript:

```js
// document.querySelectorAll(".accordion .item .header").forEach((header) => {
//     ...
// });
```

Esse código parece estar relacionado ao FAQ, mas não considerei como um dos problemas principais porque não consegui confirmar completamente a causa.

---

# Resumo

| Nº | Problema                                     | Tela/Local     | Severidade       |
| -- | -------------------------------------------- | -------------- | ---------------- |
| 1  | Erro no JavaScript dos links                 | Todas          | Crítica          |
| 2  | Botões de compra levam para 404              | Todas          | Crítica          |
| 3  | Cards maiores que a tela                     | 360px          | Crítica          |
| 4  | Texto da seção "Sobre" difícil de ler        | 768px          | Média            |
| 5  | Número da avaliação difícil de enxergar      | 768px          | Média            |
| 6  | Texto de "O que esperar" com pouco contraste | 768px          | Média            |
| 7  | Link "Contact Page" com pouco contraste      | 768px          | Média            |
| 8  | Página rola sozinha ao mudar a resolução     | Responsividade | Baixa — Suspeita | 
| 8  | Página rola sozinha ao mudar a resolução     | Responsividade | Baixa — Suspeita | 


---

# Testes realizados

### 360px

Encontrei problemas nos cards dos kits, que ultrapassam a largura da tela.

Também apareceu o erro de JavaScript no Console.

### 768px

Encontrei alguns problemas visuais relacionados ao contraste dos textos e elementos.

O erro de JavaScript também continuou aparecendo.

### 1440px

Também testei a página em 1440px para verificar o comportamento em uma tela maior.

Durante os testes de alteração da resolução, percebi o comportamento de rolagem automática citado como suspeita neste documento.

---

# Conclusão

Durante os testes encontrei problemas principalmente nos links de compra, JavaScript, responsividade e visual da página.

Os problemas que considero mais importantes são os links de compra que levam para uma página 404, o erro no JavaScript e os cards que ultrapassam a tela em 360px.

Também encontrei alguns problemas de contraste que podem ser melhorados para facilitar a leitura.

A rolagem automática ficou como uma suspeita porque percebi o comportamento, mas ainda não consegui confirmar o motivo exato.
